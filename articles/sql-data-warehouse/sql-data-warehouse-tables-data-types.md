---
title: "aaaData typer vägledning - Azure SQL Data Warehouse | Microsoft Docs"
description: "Rekommendationer toodefine datatyper som är kompatibla med SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: barbkess
editor: 
ms.assetid: d4a1f0a3-ba9f-44b9-95f6-16a4f30746d6
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 06/02/2017
ms.author: shigu;barbkess
ms.openlocfilehash: a2f7a394feb73d273b25101735b00eb12db2b292
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="guidance-for-defining-data-types-for-tables-in-sql-data-warehouse"></a><span data-ttu-id="f1b98-103">Riktlinjer för att definiera datatyper för tabeller i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="f1b98-103">Guidance for defining data types for tables in SQL Data Warehouse</span></span>
<span data-ttu-id="f1b98-104">Använd dessa rekommendationer toodefine tabell datatyper som är kompatibla med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f1b98-104">Use these recommendations toodefine table data types that are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="f1b98-105">Dessutom förbättrar toocompatibility, minimera hello storlek på datatyper för frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="f1b98-105">In addition toocompatibility, minimizing hello size of data types improves query performance.</span></span>

<span data-ttu-id="f1b98-106">SQL Data Warehouse stöder de vanligaste hello-datatyper.</span><span class="sxs-lookup"><span data-stu-id="f1b98-106">SQL Data Warehouse supports hello most commonly used data types.</span></span> <span data-ttu-id="f1b98-107">En lista över hello stöds datatyper finns [datatyper](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) i hello CREATE TABLE-instruktion.</span><span class="sxs-lookup"><span data-stu-id="f1b98-107">For a list of hello supported data types, see [data types](/sql/docs/t-sql/statements/create-table-azure-sql-data-warehouse.md#datatypes) in hello CREATE TABLE statement.</span></span> 


## <a name="minimize-row-length"></a><span data-ttu-id="f1b98-108">Minimera radlängden</span><span class="sxs-lookup"><span data-stu-id="f1b98-108">Minimize row length</span></span>
<span data-ttu-id="f1b98-109">Minimera hello storlek på datatyper för förkortar hello radlängden, vilket leder toobetter frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="f1b98-109">Minimizing hello size of data types shortens hello row length, which leads toobetter query performance.</span></span> <span data-ttu-id="f1b98-110">Använd hello minsta datatypen som fungerar för dina data.</span><span class="sxs-lookup"><span data-stu-id="f1b98-110">Use hello smallest data type that works for your data.</span></span> 

- <span data-ttu-id="f1b98-111">Undvik att definiera teckenkolumner med en stor standardlängden.</span><span class="sxs-lookup"><span data-stu-id="f1b98-111">Avoid defining character columns with a large default length.</span></span> <span data-ttu-id="f1b98-112">Till exempel om hello längsta värde är 25 tecken, definiera kolumnen som VARCHAR(25).</span><span class="sxs-lookup"><span data-stu-id="f1b98-112">For example, if hello longest value is 25 characters, then define your column as VARCHAR(25).</span></span> 
- <span data-ttu-id="f1b98-113">Undvik att använda [NVARCHAR] [ NVARCHAR] när du behöver bara VARCHAR.</span><span class="sxs-lookup"><span data-stu-id="f1b98-113">Avoid using [NVARCHAR][NVARCHAR] when you only need VARCHAR.</span></span>
- <span data-ttu-id="f1b98-114">När det är möjligt använda NVARCHAR(4000) eller VARCHAR(8000) i stället för NVARCHAR(MAX) eller VARCHAR(MAX).</span><span class="sxs-lookup"><span data-stu-id="f1b98-114">When possible, use NVARCHAR(4000) or VARCHAR(8000) instead of NVARCHAR(MAX) or VARCHAR(MAX).</span></span>

<span data-ttu-id="f1b98-115">Om du använder Polybase tooload tabellerna hello definierats hello tabellrad kan inte vara längre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="f1b98-115">If you are using Polybase tooload your tables, hello defined length of hello table row cannot exceed 1 MB.</span></span> <span data-ttu-id="f1b98-116">När en rad med variabel längd överskrider 1 MB, kan du läsa in hello raden med BCP, men inte med PolyBase.</span><span class="sxs-lookup"><span data-stu-id="f1b98-116">When a row with variable-length data exceeds 1 MB, you can load hello row with BCP, but not with PolyBase.</span></span>

## <a name="identify-unsupported-data-types"></a><span data-ttu-id="f1b98-117">Identifiera datatyper</span><span class="sxs-lookup"><span data-stu-id="f1b98-117">Identify unsupported data types</span></span>
<span data-ttu-id="f1b98-118">Om du migrerar databasen från en annan SQL-databas, kan det uppstå datatyper som inte stöds i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="f1b98-118">If you are migrating your database from another SQL database, you might encounter data types that are not supported in SQL Data Warehouse.</span></span> <span data-ttu-id="f1b98-119">Använd den här frågan toodiscover stöds inte datatyper i din befintliga SQL-schemat.</span><span class="sxs-lookup"><span data-stu-id="f1b98-119">Use this query toodiscover unsupported data types in your existing SQL schema.</span></span>

```sql
SELECT  t.[name], c.[name], c.[system_type_id], c.[user_type_id], y.[is_user_defined], y.[name]
FROM sys.tables  t
JOIN sys.columns c on t.[object_id]    = c.[object_id]
JOIN sys.types   y on c.[user_type_id] = y.[user_type_id]
WHERE y.[name] IN ('geography','geometry','hierarchyid','image','text','ntext','sql_variant','timestamp','xml')
 AND  y.[is_user_defined] = 1;
```


## <span data-ttu-id="f1b98-120"><a name="unsupported-data-types"></a>Använda lösningar för datatyper</span><span class="sxs-lookup"><span data-stu-id="f1b98-120"><a name="unsupported-data-types"></a>Use workarounds for unsupported data types</span></span>

<span data-ttu-id="f1b98-121">hello följande lista visar hello datatyper som inte har stöd för SQL Data Warehouse och ger alternativ som du kan använda i stället för hello datatyper.</span><span class="sxs-lookup"><span data-stu-id="f1b98-121">hello following list shows hello data types that SQL Data Warehouse does not support and gives alternatives that you can use instead of hello unsupported data types.</span></span>

| <span data-ttu-id="f1b98-122">Datatyp</span><span class="sxs-lookup"><span data-stu-id="f1b98-122">Unsupported data type</span></span> | <span data-ttu-id="f1b98-123">Lösning</span><span class="sxs-lookup"><span data-stu-id="f1b98-123">Workaround</span></span> |
| --- | --- |
| <span data-ttu-id="f1b98-124">[geometri][geometry]</span><span class="sxs-lookup"><span data-stu-id="f1b98-124">[geometry][geometry]</span></span> |<span data-ttu-id="f1b98-125">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="f1b98-125">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="f1b98-126">[geografisk plats][geography]</span><span class="sxs-lookup"><span data-stu-id="f1b98-126">[geography][geography]</span></span> |<span data-ttu-id="f1b98-127">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="f1b98-127">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="f1b98-128">[hierarchyid][hierarchyid]</span><span class="sxs-lookup"><span data-stu-id="f1b98-128">[hierarchyid][hierarchyid]</span></span> |<span data-ttu-id="f1b98-129">[nvarchar][nvarchar](4000)</span><span class="sxs-lookup"><span data-stu-id="f1b98-129">[nvarchar][nvarchar](4000)</span></span> |
| <span data-ttu-id="f1b98-130">[bild][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="f1b98-130">[image][ntext,text,image]</span></span> |<span data-ttu-id="f1b98-131">[varbinary][varbinary]</span><span class="sxs-lookup"><span data-stu-id="f1b98-131">[varbinary][varbinary]</span></span> |
| <span data-ttu-id="f1b98-132">[text][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="f1b98-132">[text][ntext,text,image]</span></span> |<span data-ttu-id="f1b98-133">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="f1b98-133">[varchar][varchar]</span></span> |
| <span data-ttu-id="f1b98-134">[ntext][ntext,text,image]</span><span class="sxs-lookup"><span data-stu-id="f1b98-134">[ntext][ntext,text,image]</span></span> |<span data-ttu-id="f1b98-135">[nvarchar][nvarchar]</span><span class="sxs-lookup"><span data-stu-id="f1b98-135">[nvarchar][nvarchar]</span></span> |
| <span data-ttu-id="f1b98-136">[sql_variant][sql_variant]</span><span class="sxs-lookup"><span data-stu-id="f1b98-136">[sql_variant][sql_variant]</span></span> |<span data-ttu-id="f1b98-137">Dela upp kolumn i flera strikt typkontroll kolumner.</span><span class="sxs-lookup"><span data-stu-id="f1b98-137">Split column into several strongly typed columns.</span></span> |
| <span data-ttu-id="f1b98-138">[tabell][table]</span><span class="sxs-lookup"><span data-stu-id="f1b98-138">[table][table]</span></span> |<span data-ttu-id="f1b98-139">Konvertera tootemporary tabeller.</span><span class="sxs-lookup"><span data-stu-id="f1b98-139">Convert tootemporary tables.</span></span> |
| <span data-ttu-id="f1b98-140">[tidsstämpel][timestamp]</span><span class="sxs-lookup"><span data-stu-id="f1b98-140">[timestamp][timestamp]</span></span> |<span data-ttu-id="f1b98-141">Omarbeta kod toouse [datetime2] [ datetime2] och `CURRENT_TIMESTAMP` funktion.</span><span class="sxs-lookup"><span data-stu-id="f1b98-141">Rework code toouse [datetime2][datetime2] and `CURRENT_TIMESTAMP` function.</span></span>  <span data-ttu-id="f1b98-142">Endast konstanter stöds som standard, därför current_timestamp kan inte definieras som en default-begränsning.</span><span class="sxs-lookup"><span data-stu-id="f1b98-142">Only constants are supported as defaults, therefore current_timestamp cannot be defined as a default constraint.</span></span> <span data-ttu-id="f1b98-143">Om du behöver toomigrate version radvärden från en tidsstämpelkolumn skrivna kan sedan använda [binära][BINARY](8) eller [VARBINARY][BINARY](8) för NOT NULL eller NULL version radvärden.</span><span class="sxs-lookup"><span data-stu-id="f1b98-143">If you need toomigrate row version values from a timestamp typed column, then use [BINARY][BINARY](8) or [VARBINARY][BINARY](8) for NOT NULL or NULL row version values.</span></span> |
| <span data-ttu-id="f1b98-144">[XML][xml]</span><span class="sxs-lookup"><span data-stu-id="f1b98-144">[xml][xml]</span></span> |<span data-ttu-id="f1b98-145">[varchar][varchar]</span><span class="sxs-lookup"><span data-stu-id="f1b98-145">[varchar][varchar]</span></span> |
| <span data-ttu-id="f1b98-146">[användardefinierad typ][user defined types]</span><span class="sxs-lookup"><span data-stu-id="f1b98-146">[user-defined type][user defined types]</span></span> |<span data-ttu-id="f1b98-147">Omvandla tillbaka toohello inbyggda datatyper när det är möjligt.</span><span class="sxs-lookup"><span data-stu-id="f1b98-147">Convert back toohello native data type when possible.</span></span> |
| <span data-ttu-id="f1b98-148">standardvärden</span><span class="sxs-lookup"><span data-stu-id="f1b98-148">default values</span></span> | <span data-ttu-id="f1b98-149">Standardvärden stöder literaler och endast konstanter.</span><span class="sxs-lookup"><span data-stu-id="f1b98-149">Default values support literals and constants only.</span></span>  <span data-ttu-id="f1b98-150">Icke-deterministiska uttryck eller funktioner, t.ex `GETDATE()` eller `CURRENT_TIMESTAMP`, stöds inte.</span><span class="sxs-lookup"><span data-stu-id="f1b98-150">Non-deterministic expressions or functions, such as `GETDATE()` or `CURRENT_TIMESTAMP`, are not supported.</span></span> |


## <a name="next-steps"></a><span data-ttu-id="f1b98-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f1b98-151">Next steps</span></span>
<span data-ttu-id="f1b98-152">Det finns fler toolearn:</span><span class="sxs-lookup"><span data-stu-id="f1b98-152">toolearn more, see:</span></span>

- <span data-ttu-id="f1b98-153">[Metodtips för SQL Data Warehouse][SQL Data Warehouse Best Practices]</span><span class="sxs-lookup"><span data-stu-id="f1b98-153">[SQL Data Warehouse Best Practices][SQL Data Warehouse Best Practices]</span></span>
- <span data-ttu-id="f1b98-154">[Översikt över tabell][Overview]</span><span class="sxs-lookup"><span data-stu-id="f1b98-154">[Table Overview][Overview]</span></span>
- <span data-ttu-id="f1b98-155">[Distribuera en tabell][Distribute]</span><span class="sxs-lookup"><span data-stu-id="f1b98-155">[Distributing a Table][Distribute]</span></span>
- <span data-ttu-id="f1b98-156">[Indexering av en tabell][Index]</span><span class="sxs-lookup"><span data-stu-id="f1b98-156">[Indexing a Table][Index]</span></span>
- <span data-ttu-id="f1b98-157">[Partitionering en tabell][Partition]</span><span class="sxs-lookup"><span data-stu-id="f1b98-157">[Partitioning a Table][Partition]</span></span>
- <span data-ttu-id="f1b98-158">[Underhåll av tabellstatistik][Statistics]</span><span class="sxs-lookup"><span data-stu-id="f1b98-158">[Maintaining Table Statistics][Statistics]</span></span>
- <span data-ttu-id="f1b98-159">[Temporära tabeller][Temporary]</span><span class="sxs-lookup"><span data-stu-id="f1b98-159">[Temporary Tables][Temporary]</span></span>

<!--Image references-->

<!--Article references-->
[Overview]: ./sql-data-warehouse-tables-overview.md
[Data Types]: ./sql-data-warehouse-tables-data-types.md
[Distribute]: ./sql-data-warehouse-tables-distribute.md
[Index]: ./sql-data-warehouse-tables-index.md
[Partition]: ./sql-data-warehouse-tables-partition.md
[Statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary]: ./sql-data-warehouse-tables-temporary.md
[SQL Data Warehouse Best Practices]: ./sql-data-warehouse-best-practices.md

<!--MSDN references-->

<!--Other Web references-->
[create table]: https://msdn.microsoft.com/library/mt203953.aspx
[bigint]: https://msdn.microsoft.com/library/ms187745.aspx
[binary]: https://msdn.microsoft.com/library/ms188362.aspx
[bit]: https://msdn.microsoft.com/library/ms177603.aspx
[char]: https://msdn.microsoft.com/library/ms176089.aspx
[date]: https://msdn.microsoft.com/library/bb630352.aspx
[datetime]: https://msdn.microsoft.com/library/ms187819.aspx
[datetime2]: https://msdn.microsoft.com/library/bb677335.aspx
[datetimeoffset]: https://msdn.microsoft.com/library/bb630289.aspx
[decimal]: https://msdn.microsoft.com/library/ms187746.aspx
[float]: https://msdn.microsoft.com/library/ms173773.aspx
[geometry]: https://msdn.microsoft.com/library/cc280487.aspx
[geography]: https://msdn.microsoft.com/library/cc280766.aspx
[hierarchyid]: https://msdn.microsoft.com/library/bb677290.aspx
[int]: https://msdn.microsoft.com/library/ms187745.aspx
[money]: https://msdn.microsoft.com/library/ms179882.aspx
[nchar]: https://msdn.microsoft.com/library/ms186939.aspx
[nvarchar]: https://msdn.microsoft.com/library/ms186939.aspx
[ntext,text,image]: https://msdn.microsoft.com/library/ms187993.aspx
[real]: https://msdn.microsoft.com/library/ms173773.aspx
[smalldatetime]: https://msdn.microsoft.com/library/ms182418.aspx
[smallint]: https://msdn.microsoft.com/library/ms187745.aspx
[smallmoney]: https://msdn.microsoft.com/library/ms179882.aspx
[sql_variant]: https://msdn.microsoft.com/library/ms173829.aspx
[sysname]: https://msdn.microsoft.com/library/ms186939.aspx
[table]: https://msdn.microsoft.com/library/ms175010.aspx
[time]: https://msdn.microsoft.com/library/bb677243.aspx
[timestamp]: https://msdn.microsoft.com/library/ms182776.aspx
[tinyint]: https://msdn.microsoft.com/library/ms187745.aspx
[uniqueidentifier]: https://msdn.microsoft.com/library/ms187942.aspx
[varbinary]: https://msdn.microsoft.com/library/ms188362.aspx
[varchar]: https://msdn.microsoft.com/library/ms186939.aspx
[xml]: https://msdn.microsoft.com/library/ms187339.aspx
[user defined types]: https://msdn.microsoft.com/library/ms131694.aspx
