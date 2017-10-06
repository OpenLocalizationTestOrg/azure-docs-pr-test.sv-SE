---
title: aaaLoad exempeldata i SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 3459c42f3aae51c27fd35db7874faf99e1e577e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="load-sample-data-into-sql-data-warehouse"></a><span data-ttu-id="54004-103">läs in exempeldata i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="54004-103">Load sample data into SQL Data Warehouse</span></span>
<span data-ttu-id="54004-104">Följ dessa enkla steg tooload och fråga hello Adventure Works exempeldatabasen.</span><span class="sxs-lookup"><span data-stu-id="54004-104">Follow these simple steps tooload and query hello Adventure Works Sample database.</span></span> <span data-ttu-id="54004-105">Dessa skript använder först sqlcmd toorun SQL som kommer att skapa tabeller och vyer.</span><span class="sxs-lookup"><span data-stu-id="54004-105">These scripts first use sqlcmd toorun SQL which will create tables and views.</span></span> <span data-ttu-id="54004-106">När du har skapat tabeller använder hello skript bcp tooload data.</span><span class="sxs-lookup"><span data-stu-id="54004-106">Once tables have been created, hello scripts will use bcp tooload data.</span></span>  <span data-ttu-id="54004-107">Om du inte redan har sqlcmd och bcp installerat kan du följa dessa länkar för[installera bcp] [ install bcp] och för[installera sqlcmd][install sqlcmd].</span><span class="sxs-lookup"><span data-stu-id="54004-107">If you don't already have sqlcmd and bcp installed, follow these links too[install bcp][install bcp] and too[install sqlcmd][install sqlcmd].</span></span>

## <a name="load-sample-data"></a><span data-ttu-id="54004-108">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="54004-108">Load sample data</span></span>
1. <span data-ttu-id="54004-109">Hämta hello [Adventure Works-exempelskript för SQL Data Warehouse] [ Adventure Works Sample Scripts for SQL Data Warehouse] zip-filen.</span><span class="sxs-lookup"><span data-stu-id="54004-109">Download hello [Adventure Works Sample Scripts for SQL Data Warehouse][Adventure Works Sample Scripts for SQL Data Warehouse] zip file.</span></span>
2. <span data-ttu-id="54004-110">Extrahera hello filer från hämtade zip tooa katalog på din lokala dator.</span><span class="sxs-lookup"><span data-stu-id="54004-110">Extract hello files from downloaded zip tooa directory on your local machine.</span></span>
3. <span data-ttu-id="54004-111">Redigera hello extraheras filen aw_create.bat och ange följande variabler som finns hello överst i filen hello hello.</span><span class="sxs-lookup"><span data-stu-id="54004-111">Edit hello extracted file aw_create.bat and set hello following variables found at hello top of hello file.</span></span>  <span data-ttu-id="54004-112">Vara säker på att tooleave inga blanksteg mellan hello ”=” och hello-parametern.</span><span class="sxs-lookup"><span data-stu-id="54004-112">Be sure tooleave no whitespace between hello "=" and hello parameter.</span></span>  <span data-ttu-id="54004-113">Nedan följer exempel på hur redigeringarna kan se ut.</span><span class="sxs-lookup"><span data-stu-id="54004-113">Below are examples of how your edits might look.</span></span>
   
    ```
    server=mylogicalserver.database.windows.net
    user=mydwuser
    password=Mydwpassw0rd
    database=mydwdatabase
    ```
4. <span data-ttu-id="54004-114">Kör hello redigeras aw_create.bat från en Windows-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="54004-114">From a Windows cmd prompt, run hello edited aw_create.bat.</span></span>  <span data-ttu-id="54004-115">Var noga med att du befinner dig i hello katalog där du sparade den redigerade versionen av aw_create.bat.</span><span class="sxs-lookup"><span data-stu-id="54004-115">Be sure you are in hello directory where you saved your edited version of aw_create.bat.</span></span>
   <span data-ttu-id="54004-116">Det här skriptet kommer...</span><span class="sxs-lookup"><span data-stu-id="54004-116">This script will...</span></span>
   
   * <span data-ttu-id="54004-117">Ta bort alla Adventure Works tabeller eller vyer som redan finns i databasen</span><span class="sxs-lookup"><span data-stu-id="54004-117">Drop any Adventure Works tables or views that already exist in your database</span></span>
   * <span data-ttu-id="54004-118">Skapa hello Adventure Works tabeller och vyer</span><span class="sxs-lookup"><span data-stu-id="54004-118">Create hello Adventure Works tables and views</span></span>
   * <span data-ttu-id="54004-119">Läsa in varje Adventure Works-tabell med bcp</span><span class="sxs-lookup"><span data-stu-id="54004-119">Load each Adventure Works table using bcp</span></span>
   * <span data-ttu-id="54004-120">Validera hello radantal för varje Adventure Works-tabell</span><span class="sxs-lookup"><span data-stu-id="54004-120">Validate hello row counts for each Adventure Works table</span></span>
   * <span data-ttu-id="54004-121">Samla in statistik på varje kolumn för varje Adventure Works-tabell</span><span class="sxs-lookup"><span data-stu-id="54004-121">Collect statistics on every column for each Adventure Works table</span></span>

## <a name="query-sample-data"></a><span data-ttu-id="54004-122">Fråga exempeldata</span><span class="sxs-lookup"><span data-stu-id="54004-122">Query sample data</span></span>
<span data-ttu-id="54004-123">När du har läst in exempeldata i SQL Data Warehouse, kan du snabbt köra några frågor.</span><span class="sxs-lookup"><span data-stu-id="54004-123">Once you've loaded some sample data into your SQL Data Warehouse, you can quickly run a few queries.</span></span>  <span data-ttu-id="54004-124">toorun en fråga ansluta tooyour nyskapad Adventure Works-databasen i Azure SQL DW med hjälp av Visual Studio och SSDT, enligt beskrivningen i hello [fråga med Visual Studio] [ query with Visual Studio] dokumentet.</span><span class="sxs-lookup"><span data-stu-id="54004-124">toorun a query, connect tooyour newly created Adventure Works database in Azure SQL DW using Visual Studio and SSDT, as described in hello [query with Visual Studio][query with Visual Studio] document.</span></span>

<span data-ttu-id="54004-125">Exempel på enkla Välj instruktionen tooget alla hello info för hello anställda:</span><span class="sxs-lookup"><span data-stu-id="54004-125">Example of simple select statement tooget all hello info of hello employees:</span></span>

```sql
SELECT * FROM DimEmployee;
```

<span data-ttu-id="54004-126">Exempel på en mer komplex fråga med konstruktioner, till exempel GROUP BY toolook på hello totalbelopp för all försäljning varje dag:</span><span class="sxs-lookup"><span data-stu-id="54004-126">Example of a more complex query using constructs such as GROUP BY toolook at hello total amount for all sales on each day:</span></span>

```sql
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="54004-127">Exempel på en väljer med en WHERE-satsen toofilter ut order från före ett visst datum:</span><span class="sxs-lookup"><span data-stu-id="54004-127">Example of a SELECT with a WHERE clause toofilter out orders from before a certain date:</span></span>

```
SELECT OrderDateKey, SUM(SalesAmount) AS TotalSales
FROM FactInternetSales
WHERE OrderDateKey > '20020801'
GROUP BY OrderDateKey
ORDER BY OrderDateKey;
```

<span data-ttu-id="54004-128">SQL Data Warehouse stöder nästan alla T-SQL-konstruktioner som har stöd för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="54004-128">SQL Data Warehouse supports almost all T-SQL constructs which SQL Server supports.</span></span>  <span data-ttu-id="54004-129">Eventuella skillnader som finns dokumenterade i vår [Migrera koden] [ migrate code] dokumentation.</span><span class="sxs-lookup"><span data-stu-id="54004-129">Any differences are documented in our [migrate code][migrate code] documentation.</span></span>

## <a name="next-steps"></a><span data-ttu-id="54004-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="54004-130">Next steps</span></span>
<span data-ttu-id="54004-131">Nu när du har haft en chansen tootry några frågor med exempeldata, kolla hur för[utveckla][develop], [ladda][load], eller [ Migrera] [ migrate] tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="54004-131">Now that you've had a chance tootry some queries with sample data, check out how too[develop][develop], [load][load], or [migrate][migrate] tooSQL Data Warehouse.</span></span>

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
