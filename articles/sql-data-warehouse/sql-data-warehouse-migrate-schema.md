---
title: Migrera schemat till SQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera schemat till Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 538b60c9-a07f-49bf-9ea3-1082ed6699fb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 07ca2321852e276502187e768177e7e82bdfd080
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-schemas-to-sql-data-warehouse"></a><span data-ttu-id="5ef33-103">Migrera dina scheman till SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="5ef33-103">Migrate your schemas to SQL Data Warehouse</span></span>
<span data-ttu-id="5ef33-104">Riktlinjer för att migrera dina SQL-scheman till SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5ef33-104">Guidance for migrating your SQL schemas to SQL Data Warehouse.</span></span> 

## <a name="plan-your-schema-migration"></a><span data-ttu-id="5ef33-105">Planera migrering av schemat</span><span class="sxs-lookup"><span data-stu-id="5ef33-105">Plan your schema migration</span></span>

<span data-ttu-id="5ef33-106">När du planerar en migrering finns i [tabell översikt] [ table overview] att bekanta dig med tabellen designöverväganden, till exempel statistik, distribution, partitionering och indexering.</span><span class="sxs-lookup"><span data-stu-id="5ef33-106">As you plan a migration, see the [table overview][table overview] to become familiar with table design considerations such as statistics, distribution, partitioning, and indexing.</span></span>  <span data-ttu-id="5ef33-107">Den listar också några [stöds inte i tabellen funktioner] [ unsupported table features] och sina lösningar.</span><span class="sxs-lookup"><span data-stu-id="5ef33-107">It also lists some [unsupported table features][unsupported table features] and their workarounds.</span></span>

## <a name="use-user-defined-schemas-to-consolidate-databases"></a><span data-ttu-id="5ef33-108">Användardefinierade scheman används för att konsolidera databaser</span><span class="sxs-lookup"><span data-stu-id="5ef33-108">Use user-defined schemas to consolidate databases</span></span>

<span data-ttu-id="5ef33-109">Din befintliga arbetsbelastning har antagligen mer än en databas.</span><span class="sxs-lookup"><span data-stu-id="5ef33-109">Your existing workload probably has more than one database.</span></span> <span data-ttu-id="5ef33-110">Ett informationslager i SQL Server kan exempelvis innehålla en fristående databas, en databas för datalager och vissa data mart-databaser.</span><span class="sxs-lookup"><span data-stu-id="5ef33-110">For example, a SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="5ef33-111">I den här topologin körs varje databas som en separat arbetsbelastning med separata säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="5ef33-111">In this topology, each database runs as a separate workload with separate security policies.</span></span>

<span data-ttu-id="5ef33-112">Däremot kör SQL Data Warehouse hela arbetsbelastningen för informationslager inom en databas.</span><span class="sxs-lookup"><span data-stu-id="5ef33-112">By contrast, SQL Data Warehouse runs the entire data warehouse workload within one database.</span></span> <span data-ttu-id="5ef33-113">Mellan databasen tillåts inte kopplingar.</span><span class="sxs-lookup"><span data-stu-id="5ef33-113">Cross database joins are not permitted.</span></span> <span data-ttu-id="5ef33-114">SQL Data Warehouse förväntar därför alla tabeller som används i datalagret lagras i en databas.</span><span class="sxs-lookup"><span data-stu-id="5ef33-114">Therefore, SQL Data Warehouse expects all tables used by the data warehouse to be stored within the one database.</span></span>

<span data-ttu-id="5ef33-115">Vi rekommenderar att du använder användardefinierade scheman för att konsolidera din befintliga arbetsbelastning i en databas.</span><span class="sxs-lookup"><span data-stu-id="5ef33-115">We recommend using user-defined schemas to consolidate your existing workload into one database.</span></span> <span data-ttu-id="5ef33-116">Exempel finns [användardefinierade scheman](sql-data-warehouse-develop-user-defined-schemas.md)</span><span class="sxs-lookup"><span data-stu-id="5ef33-116">For examples, see [User-defined schemas](sql-data-warehouse-develop-user-defined-schemas.md)</span></span>

## <a name="use-compatible-data-types"></a><span data-ttu-id="5ef33-117">Använd kompatibla datatyper</span><span class="sxs-lookup"><span data-stu-id="5ef33-117">Use compatible data types</span></span>
<span data-ttu-id="5ef33-118">Ändra datatyper för att vara kompatibel med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5ef33-118">Modify your data types to be compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="5ef33-119">En lista över datatyper som stöds respektive inte stöds, se [datatyper][data types].</span><span class="sxs-lookup"><span data-stu-id="5ef33-119">For a list of supported and unsupported data types, see [data types][data types].</span></span> <span data-ttu-id="5ef33-120">Avsnittet ger lösningar för typer som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="5ef33-120">That topic gives workarounds for the unsupported types.</span></span> <span data-ttu-id="5ef33-121">Det ger också en fråga för att identifiera befintliga typer som inte stöds i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="5ef33-121">It also provides a query to identify existing types that are not supported in SQL Data Warehouse.</span></span>

## <a name="minimize-row-size"></a><span data-ttu-id="5ef33-122">Minimera Radstorleken</span><span class="sxs-lookup"><span data-stu-id="5ef33-122">Minimize row size</span></span>
<span data-ttu-id="5ef33-123">Minimera radlängden tabeller för bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="5ef33-123">For best performance, minimize the row length of your tables.</span></span> <span data-ttu-id="5ef33-124">Eftersom kortare raden längder leda till bättre prestanda, kan du använda de minsta datatyper som fungerar för dina data.</span><span class="sxs-lookup"><span data-stu-id="5ef33-124">Since shorter row lengths lead to better performance, use the smallest data types that work for your data.</span></span> 

<span data-ttu-id="5ef33-125">Tabellens rad bredd har PolyBase en gräns på 1 MB.</span><span class="sxs-lookup"><span data-stu-id="5ef33-125">For table row width, PolyBase has a 1 MB limit.</span></span>  <span data-ttu-id="5ef33-126">Om du planerar att läsa in data till SQL Data Warehouse med PolyBase, uppdatera tabellerna om du vill ha maximal raden bredden på mindre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="5ef33-126">If you plan to load data into SQL Data Warehouse with PolyBase, update your tables to have maximum row widths of less than 1 MB.</span></span> 

<!--
- For example, this table uses variable length data but the largest possible size of the row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and the defined row width is less than one MB. When loading rows, PolyBase allocates the full length of the variable-length data. The full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-the-distribution-option"></a><span data-ttu-id="5ef33-127">Ange distributionsalternativ för</span><span class="sxs-lookup"><span data-stu-id="5ef33-127">Specify the distribution option</span></span>
<span data-ttu-id="5ef33-128">SQL Data Warehouse är en distribuerad databas.</span><span class="sxs-lookup"><span data-stu-id="5ef33-128">SQL Data Warehouse is a distributed database system.</span></span> <span data-ttu-id="5ef33-129">Varje tabell distribueras eller replikeras över Compute-noder.</span><span class="sxs-lookup"><span data-stu-id="5ef33-129">Each table is distributed or replicated across the Compute nodes.</span></span> <span data-ttu-id="5ef33-130">Det finns ett Tabellalternativ som du kan ange hur du distribuerar data.</span><span class="sxs-lookup"><span data-stu-id="5ef33-130">There's a table option that lets you specify how to distribute the data.</span></span> <span data-ttu-id="5ef33-131">Alternativen är resursallokering, replikeras, eller hash distribueras.</span><span class="sxs-lookup"><span data-stu-id="5ef33-131">The choices are  round-robin, replicated, or hash distributed.</span></span> <span data-ttu-id="5ef33-132">Varje har- och nackdelar.</span><span class="sxs-lookup"><span data-stu-id="5ef33-132">Each has pros and cons.</span></span> <span data-ttu-id="5ef33-133">Om du inte anger distributionsalternativet använder SQL Data Warehouse resursallokering som standard.</span><span class="sxs-lookup"><span data-stu-id="5ef33-133">If you don't specify the distribution option, SQL Data Warehouse will use round-robin as the default.</span></span>

- <span data-ttu-id="5ef33-134">Resursallokering är standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="5ef33-134">Round-robin is the default.</span></span> <span data-ttu-id="5ef33-135">Den är enkel att använda och belastningar data så snabbt som möjligt och kopplingar kräver dataflyttning som minskar frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="5ef33-135">It is the simplest to use, and loads the data as fast as possible, but joins will require data movement which slows query performance.</span></span>
- <span data-ttu-id="5ef33-136">Replikerade lagrar en kopia av tabellen på varje Compute-nod.</span><span class="sxs-lookup"><span data-stu-id="5ef33-136">Replicated stores a copy of the table on each Compute node.</span></span> <span data-ttu-id="5ef33-137">Replikerade tabeller är performant eftersom de inte kräver dataflyttning för kopplingar och aggregeringar.</span><span class="sxs-lookup"><span data-stu-id="5ef33-137">Replicated tables are performant because they do not require data movement for joins and aggregations.</span></span> <span data-ttu-id="5ef33-138">De kräver extra lagring och därför fungerar bäst för mindre tabeller.</span><span class="sxs-lookup"><span data-stu-id="5ef33-138">They do require extra storage, and therefore work best for smaller tables.</span></span>
- <span data-ttu-id="5ef33-139">Hash distribuerade distribuerar rader för alla noder via en hash-funktionen.</span><span class="sxs-lookup"><span data-stu-id="5ef33-139">Hash distributed distributes the rows across all the nodes via a hash function.</span></span> <span data-ttu-id="5ef33-140">Distribuerad hash-tabeller är hjärtat i SQL Data Warehouse eftersom de har utformats för att tillhandahålla hög frågeprestanda för stora tabeller.</span><span class="sxs-lookup"><span data-stu-id="5ef33-140">Hash distributed tables are the heart of SQL Data Warehouse since they are designed to provide high query performance on large tables.</span></span> <span data-ttu-id="5ef33-141">Det här alternativet kräver vissa planerar att välja den bästa kolumn som ska distribuera data.</span><span class="sxs-lookup"><span data-stu-id="5ef33-141">This option requires some planning to select the best column on which to distribute the data.</span></span> <span data-ttu-id="5ef33-142">Men om du inte väljer den bästa kolumnen första gången, kan du enkelt nytt distribuera data på en annan kolumn.</span><span class="sxs-lookup"><span data-stu-id="5ef33-142">However, if you don't choose the best column the first time, you can easily re-distribute the data on a different column.</span></span> 

<span data-ttu-id="5ef33-143">Om du vill välja den bästa distributionsalternativ för varje tabell, se [distribuerade tabeller](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="5ef33-143">To choose the best distribution option for each table, see [Distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="5ef33-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5ef33-144">Next steps</span></span>
<span data-ttu-id="5ef33-145">När du har migrerat dina databasschemat till SQL Data Warehouse, kan du gå vidare till någon av följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="5ef33-145">Once you have successfully migrated your database schema to SQL Data Warehouse, proceed to one of the following articles:</span></span>

* <span data-ttu-id="5ef33-146">[Migrera dina data][Migrate your data]</span><span class="sxs-lookup"><span data-stu-id="5ef33-146">[Migrate your data][Migrate your data]</span></span>
* <span data-ttu-id="5ef33-147">[Migrera koden][Migrate your code]</span><span class="sxs-lookup"><span data-stu-id="5ef33-147">[Migrate your code][Migrate your code]</span></span>

<span data-ttu-id="5ef33-148">Mer information om metodtips för SQL Data Warehouse, finns det [metodtips] [ best practices] artikel.</span><span class="sxs-lookup"><span data-stu-id="5ef33-148">For more about SQL Data Warehouse best practices, see the [best practices][best practices] article.</span></span>

<!--Image references-->

<!--Article references-->
[Migrate your code]: ./sql-data-warehouse-migrate-code.md
[Migrate your data]: ./sql-data-warehouse-migrate-data.md
[best practices]: ./sql-data-warehouse-best-practices.md
[table overview]: ./sql-data-warehouse-tables-overview.md
[unsupported table features]: ./sql-data-warehouse-tables-overview.md#unsupported-table-features
[data types]: ./sql-data-warehouse-tables-data-types.md
[unsupported data types]: ./sql-data-warehouse-tables-data-types.md#unsupported-data-types

<!--MSDN references-->


<!--Other Web references-->
