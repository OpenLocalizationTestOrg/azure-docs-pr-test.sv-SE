---
title: "Läs in exempeldata i SQL Data Warehouse | Microsoft Docs"
description: "läs in exempeldata i SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: ckarst
manager: jhubbard
editor: 
ms.assetid: e338ecf8-cfee-419b-b7b6-98108d381c62
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: loading
ms.date: 10/31/2016
ms.author: cakarst;barbkess
ms.openlocfilehash: 1e0df958a2f18fe1e988168918e5cfd293f84e64
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a><span data-ttu-id="095b1-103">läs in exempeldata i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="095b1-103">Load sample data into SQL Data Warehouse</span></span>
<span data-ttu-id="095b1-104">Följ dessa enkla steg för att läsa in och fråga exempeldatabasen för Adventure Works.</span><span class="sxs-lookup"><span data-stu-id="095b1-104">Follow these simple steps to load and query the Adventure Works Sample database.</span></span> <span data-ttu-id="095b1-105">Dessa skript använder sqlcmd först för att köra SQL som kommer att skapa tabeller och vyer.</span><span class="sxs-lookup"><span data-stu-id="095b1-105">These scripts first use sqlcmd to run SQL which will create tables and views.</span></span> <span data-ttu-id="095b1-106">När tabeller har skapats, använder skripten bcp för att läsa in data.</span><span class="sxs-lookup"><span data-stu-id="095b1-106">Once tables have been created, the scripts will use bcp to load data.</span></span>  <span data-ttu-id="095b1-107">Om du inte redan har sqlcmd och bcp installerat kan du följa dessa länkar till [installera bcp] [ install bcp] och [installera sqlcmd][install sqlcmd].</span><span class="sxs-lookup"><span data-stu-id="095b1-107">If you don't already have sqlcmd and bcp installed, follow these links to [install bcp][install bcp] and to [install sqlcmd][install sqlcmd].</span></span>

## <a name="load-sample-data"></a><span data-ttu-id="095b1-108">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="095b1-108">Load sample data</span></span>
1. <span data-ttu-id="095b1-109">Hämta den [Adventure Works-exempelskript för SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] zip-filen.</span><span class="sxs-lookup"><span data-stu-id="095b1-109">Download the [Adventure Works Sample Scripts for SQL Data Warehouse][Adventure Works Sample Scripts for SQL Data Warehouse] zip file.</span></span>
2. <span data-ttu-id="095b1-110">Extrahera filerna från hämtade zip till en katalog på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="095b1-110">Extract the files from downloaded zip to a directory on your local machine.</span></span>
3. <span data-ttu-id="095b1-111">Redigera den hämtade filen aw_create.bat och ange följande variabler som finns längst upp i filen.</span><span class="sxs-lookup"><span data-stu-id="095b1-111">Edit the extracted file aw_create.bat and set the following variables found at the top of the file.</span></span>  <span data-ttu-id="095b1-112">Se till att lämna inga blanksteg mellan den ”=” och parametern.</span><span class="sxs-lookup"><span data-stu-id="095b1-112">Be sure to leave no whitespace between the "=" and the parameter.</span></span>  <span data-ttu-id="095b1-113">Nedan följer exempel på hur redigeringarna kan se ut.</span><span class="sxs-lookup"><span data-stu-id="095b1-113">Below are examples of how your edits might look.</span></span>
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. <span data-ttu-id="095b1-114">Kör den redigerade aw_create.bat från en Windows-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="095b1-114">From a Windows cmd prompt, run the edited aw_create.bat.</span></span>  <span data-ttu-id="095b1-115">Var noga med att du är i katalogen där du sparade den redigerade versionen av aw_create.bat.</span><span class="sxs-lookup"><span data-stu-id="095b1-115">Be sure you are in the directory where you saved your edited version of aw_create.bat.</span></span>
   <span data-ttu-id="095b1-116">Det här skriptet kommer...</span><span class="sxs-lookup"><span data-stu-id="095b1-116">This script will...</span></span>
   
   * <span data-ttu-id="095b1-117">Ta bort alla Adventure Works tabeller eller vyer som redan finns i databasen</span><span class="sxs-lookup"><span data-stu-id="095b1-117">Drop any Adventure Works tables or views that already exist in your database</span></span>
   * <span data-ttu-id="095b1-118">Skapa Adventure Works-tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="095b1-118">Create the Adventure Works tables and views</span></span>
   * <span data-ttu-id="095b1-119">Läsa in varje Adventure Works-tabell med bcp</span><span class="sxs-lookup"><span data-stu-id="095b1-119">Load each Adventure Works table using bcp</span></span>
   * <span data-ttu-id="095b1-120">Validera radantal för varje Adventure Works-tabell</span><span class="sxs-lookup"><span data-stu-id="095b1-120">Validate the row counts for each Adventure Works table</span></span>
   * <span data-ttu-id="095b1-121">Samla in statistik på varje kolumn för varje Adventure Works-tabell</span><span class="sxs-lookup"><span data-stu-id="095b1-121">Collect statistics on every column for each Adventure Works table</span></span>

## <a name="query-sample-data"></a><span data-ttu-id="095b1-122">Fråga exempeldata</span><span class="sxs-lookup"><span data-stu-id="095b1-122">Query sample data</span></span>
<span data-ttu-id="095b1-123">När du har läst in exempeldata i SQL Data Warehouse, kan du snabbt köra några frågor.</span><span class="sxs-lookup"><span data-stu-id="095b1-123">Once you've loaded some sample data into your SQL Data Warehouse, you can quickly run a few queries.</span></span>  <span data-ttu-id="095b1-124">Om du vill köra en fråga, ansluta till din nyskapade Adventure Works-databas i Azure SQL DW med hjälp av Visual Studio och SSDT, enligt beskrivningen i den [fråga med Visual Studio] [ query with Visual Studio] dokumentet.</span><span class="sxs-lookup"><span data-stu-id="095b1-124">To run a query, connect to your newly created Adventure Works database in Azure SQL DW using Visual Studio and SSDT, as described in the [query with Visual Studio][query with Visual Studio] document.</span></span>

<span data-ttu-id="095b1-125">Exempel på enkla select-instruktion att hämta informationen som anställda:</span><span class="sxs-lookup"><span data-stu-id="095b1-125">Example of simple select statement to get all the info of the employees:</span></span>

```sql
SELECT * FROM DimEmployee;
```

<span data-ttu-id="095b1-126">Exempel på en mer komplex fråga med konstruktioner, till exempel GRUPPEN genom att titta på totalsumman för alla försäljning varje dag:</span><span class="sxs-lookup"><span data-stu-id="095b1-126">Example of a more complex query using constructs such as GROUP BY to look at the total amount for all sales on each day:</span></span>

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="095b1-127">Exempel på en väljer med en WHERE-sats för att filtrera ut order från före ett visst datum:</span><span class="sxs-lookup"><span data-stu-id="095b1-127">Example of a SELECT with a WHERE clause to filter out orders from before a certain date:</span></span>

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="095b1-128">SQL Data Warehouse stöder nästan alla T-SQL-konstruktioner som har stöd för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="095b1-128">SQL Data Warehouse supports almost all T-SQL constructs which SQL Server supports.</span></span>  <span data-ttu-id="095b1-129">Eventuella skillnader som finns dokumenterade i vår [Migrera koden] [ migrate code] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="095b1-129">Any differences are documented in our [migrate code][migrate code] documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="095b1-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="095b1-130">Next steps</span></span>
<span data-ttu-id="095b1-131">Nu när du har haft möjlighet att prova några frågor med exempeldata, kolla hur man [utveckla][develop], [ladda][load], eller [ Migrera] [ migrate] till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="095b1-131">Now that you've had a chance to try some queries with sample data, check out how to [develop][develop], [load][load], or [migrate][migrate] to SQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->
[migrate]: sql-data-warehouse-overview-migrate.md
[develop]: sql-data-warehouse-overview-develop.md
[load]: sql-data-warehouse-overview-load.md
[query with Visual Studio]: sql-data-warehouse-query-visual-studio.md
[migrate code]: sql-data-warehouse-migrate-code.md
[install bcp]: sql-data-warehouse-load-with-bcp.md
[install sqlcmd]: sql-data-warehouse-get-started-connect-sqlcmd.md

<!--Other Web references-->
[Adventure Works Sample Scripts for SQL Data Warehouse]: https://migrhoststorage.blob.core.windows.net/sqldwsample/AdventureWorksSQLDW2012.zip
