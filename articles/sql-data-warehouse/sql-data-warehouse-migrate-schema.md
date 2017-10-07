---
title: aaaMigrate ditt schema tooSQL Data Warehouse | Microsoft Docs
description: "Tips för att migrera din schemat tooAzure SQL Data Warehouse för utveckling av lösningar."
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
ms.openlocfilehash: 1309b743b78564575695038a4856d9d25a2b18d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-schemas-toosql-data-warehouse"></a><span data-ttu-id="99fce-103">Migrera din scheman tooSQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="99fce-103">Migrate your schemas tooSQL Data Warehouse</span></span>
<span data-ttu-id="99fce-104">Riktlinjer för att migrera dina SQL-scheman tooSQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="99fce-104">Guidance for migrating your SQL schemas tooSQL Data Warehouse.</span></span> 

## <a name="plan-your-schema-migration"></a><span data-ttu-id="99fce-105">Planera migrering av schemat</span><span class="sxs-lookup"><span data-stu-id="99fce-105">Plan your schema migration</span></span>

<span data-ttu-id="99fce-106">När du planerar en migrering finns hello [tabell översikt] [ table overview] toobecome bekant med tabellen designöverväganden, till exempel statistik, distribution, partitionering och indexering.</span><span class="sxs-lookup"><span data-stu-id="99fce-106">As you plan a migration, see hello [table overview][table overview] toobecome familiar with table design considerations such as statistics, distribution, partitioning, and indexing.</span></span>  <span data-ttu-id="99fce-107">Den listar också några [stöds inte i tabellen funktioner] [ unsupported table features] och sina lösningar.</span><span class="sxs-lookup"><span data-stu-id="99fce-107">It also lists some [unsupported table features][unsupported table features] and their workarounds.</span></span>

## <a name="use-user-defined-schemas-tooconsolidate-databases"></a><span data-ttu-id="99fce-108">Använda anpassade scheman tooconsolidate databaser</span><span class="sxs-lookup"><span data-stu-id="99fce-108">Use user-defined schemas tooconsolidate databases</span></span>

<span data-ttu-id="99fce-109">Din befintliga arbetsbelastning har antagligen mer än en databas.</span><span class="sxs-lookup"><span data-stu-id="99fce-109">Your existing workload probably has more than one database.</span></span> <span data-ttu-id="99fce-110">Ett informationslager i SQL Server kan exempelvis innehålla en fristående databas, en databas för datalager och vissa data mart-databaser.</span><span class="sxs-lookup"><span data-stu-id="99fce-110">For example, a SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="99fce-111">I den här topologin körs varje databas som en separat arbetsbelastning med separata säkerhetsprinciper.</span><span class="sxs-lookup"><span data-stu-id="99fce-111">In this topology, each database runs as a separate workload with separate security policies.</span></span>

<span data-ttu-id="99fce-112">Däremot kör SQL Data Warehouse hello hela arbetsbelastningen för informationslager inom en databas.</span><span class="sxs-lookup"><span data-stu-id="99fce-112">By contrast, SQL Data Warehouse runs hello entire data warehouse workload within one database.</span></span> <span data-ttu-id="99fce-113">Mellan databasen tillåts inte kopplingar.</span><span class="sxs-lookup"><span data-stu-id="99fce-113">Cross database joins are not permitted.</span></span> <span data-ttu-id="99fce-114">SQL Data Warehouse förväntar därför alla tabeller som används av hello data warehouse toobe lagras i en hello-databas.</span><span class="sxs-lookup"><span data-stu-id="99fce-114">Therefore, SQL Data Warehouse expects all tables used by hello data warehouse toobe stored within hello one database.</span></span>

<span data-ttu-id="99fce-115">Vi rekommenderar att du använder användardefinierade scheman tooconsolidate din befintliga arbetsbelastning i en databas.</span><span class="sxs-lookup"><span data-stu-id="99fce-115">We recommend using user-defined schemas tooconsolidate your existing workload into one database.</span></span> <span data-ttu-id="99fce-116">Exempel finns [användardefinierade scheman](sql-data-warehouse-develop-user-defined-schemas.md)</span><span class="sxs-lookup"><span data-stu-id="99fce-116">For examples, see [User-defined schemas](sql-data-warehouse-develop-user-defined-schemas.md)</span></span>

## <a name="use-compatible-data-types"></a><span data-ttu-id="99fce-117">Använd kompatibla datatyper</span><span class="sxs-lookup"><span data-stu-id="99fce-117">Use compatible data types</span></span>
<span data-ttu-id="99fce-118">Ändra dina data typer toobe kompatibel med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="99fce-118">Modify your data types toobe compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="99fce-119">En lista över datatyper som stöds respektive inte stöds, se [datatyper][data types].</span><span class="sxs-lookup"><span data-stu-id="99fce-119">For a list of supported and unsupported data types, see [data types][data types].</span></span> <span data-ttu-id="99fce-120">Avsnittet ger lösningar för typer hello stöds inte.</span><span class="sxs-lookup"><span data-stu-id="99fce-120">That topic gives workarounds for hello unsupported types.</span></span> <span data-ttu-id="99fce-121">Det ger också en fråga tooidentify befintliga typer som inte stöds i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="99fce-121">It also provides a query tooidentify existing types that are not supported in SQL Data Warehouse.</span></span>

## <a name="minimize-row-size"></a><span data-ttu-id="99fce-122">Minimera Radstorleken</span><span class="sxs-lookup"><span data-stu-id="99fce-122">Minimize row size</span></span>
<span data-ttu-id="99fce-123">Minimera hello radlängden tabeller för bästa prestanda.</span><span class="sxs-lookup"><span data-stu-id="99fce-123">For best performance, minimize hello row length of your tables.</span></span> <span data-ttu-id="99fce-124">Eftersom kortare raden längder leda toobetter prestanda, kan du använda hello minsta datatyper som fungerar för dina data.</span><span class="sxs-lookup"><span data-stu-id="99fce-124">Since shorter row lengths lead toobetter performance, use hello smallest data types that work for your data.</span></span> 

<span data-ttu-id="99fce-125">Tabellens rad bredd har PolyBase en gräns på 1 MB.</span><span class="sxs-lookup"><span data-stu-id="99fce-125">For table row width, PolyBase has a 1 MB limit.</span></span>  <span data-ttu-id="99fce-126">Om du planerar tooload data till SQL Data Warehouse med PolyBase uppdatera dina tabeller toohave största raden bredden på mindre än 1 MB.</span><span class="sxs-lookup"><span data-stu-id="99fce-126">If you plan tooload data into SQL Data Warehouse with PolyBase, update your tables toohave maximum row widths of less than 1 MB.</span></span> 

<!--
- For example, this table uses variable length data but hello largest possible size of hello row is still less than 1 MB. PolyBase will load data into this table.

- This table uses variable length data and hello defined row width is less than one MB. When loading rows, PolyBase allocates hello full length of hello variable-length data. hello full length of this row is greater than one MB.  PolyBase will not load data into this table.  

-->

## <a name="specify-hello-distribution-option"></a><span data-ttu-id="99fce-127">Ange hello distributionsalternativ</span><span class="sxs-lookup"><span data-stu-id="99fce-127">Specify hello distribution option</span></span>
<span data-ttu-id="99fce-128">SQL Data Warehouse är en distribuerad databas.</span><span class="sxs-lookup"><span data-stu-id="99fce-128">SQL Data Warehouse is a distributed database system.</span></span> <span data-ttu-id="99fce-129">Varje tabell distribueras eller replikeras över hello Compute-noder.</span><span class="sxs-lookup"><span data-stu-id="99fce-129">Each table is distributed or replicated across hello Compute nodes.</span></span> <span data-ttu-id="99fce-130">Det finns ett Tabellalternativ som du kan ange hur toodistribute hello data.</span><span class="sxs-lookup"><span data-stu-id="99fce-130">There's a table option that lets you specify how toodistribute hello data.</span></span> <span data-ttu-id="99fce-131">hello alternativ är resursallokering, replikeras, eller hash distribueras.</span><span class="sxs-lookup"><span data-stu-id="99fce-131">hello choices are  round-robin, replicated, or hash distributed.</span></span> <span data-ttu-id="99fce-132">Varje har- och nackdelar.</span><span class="sxs-lookup"><span data-stu-id="99fce-132">Each has pros and cons.</span></span> <span data-ttu-id="99fce-133">Om du inte anger hello distributionsalternativ använder SQL Data Warehouse resursallokering som hello standard.</span><span class="sxs-lookup"><span data-stu-id="99fce-133">If you don't specify hello distribution option, SQL Data Warehouse will use round-robin as hello default.</span></span>

- <span data-ttu-id="99fce-134">Resursallokering är hello som standard.</span><span class="sxs-lookup"><span data-stu-id="99fce-134">Round-robin is hello default.</span></span> <span data-ttu-id="99fce-135">Är det enklaste hello-toouse och läser in hello data så snabbt som möjligt, men kopplingar kräver dataflyttning som minskar frågeprestanda.</span><span class="sxs-lookup"><span data-stu-id="99fce-135">It is hello simplest toouse, and loads hello data as fast as possible, but joins will require data movement which slows query performance.</span></span>
- <span data-ttu-id="99fce-136">Replikerade lagrar en kopia av hello tabellen på varje beräkningsnod.</span><span class="sxs-lookup"><span data-stu-id="99fce-136">Replicated stores a copy of hello table on each Compute node.</span></span> <span data-ttu-id="99fce-137">Replikerade tabeller är performant eftersom de inte kräver dataflyttning för kopplingar och aggregeringar.</span><span class="sxs-lookup"><span data-stu-id="99fce-137">Replicated tables are performant because they do not require data movement for joins and aggregations.</span></span> <span data-ttu-id="99fce-138">De kräver extra lagring och därför fungerar bäst för mindre tabeller.</span><span class="sxs-lookup"><span data-stu-id="99fce-138">They do require extra storage, and therefore work best for smaller tables.</span></span>
- <span data-ttu-id="99fce-139">Hash distribuerade distribuerar hello rader för alla hello noder via en hash-funktionen.</span><span class="sxs-lookup"><span data-stu-id="99fce-139">Hash distributed distributes hello rows across all hello nodes via a hash function.</span></span> <span data-ttu-id="99fce-140">Distribuerade hash-tabeller är hello hjärtat i SQL Data Warehouse eftersom de är utformad tooprovide hög frågeprestanda för stora tabeller.</span><span class="sxs-lookup"><span data-stu-id="99fce-140">Hash distributed tables are hello heart of SQL Data Warehouse since they are designed tooprovide high query performance on large tables.</span></span> <span data-ttu-id="99fce-141">Det här alternativet kräver vissa planering tooselect hello bästa kolumn på vilka toodistribute hello data.</span><span class="sxs-lookup"><span data-stu-id="99fce-141">This option requires some planning tooselect hello best column on which toodistribute hello data.</span></span> <span data-ttu-id="99fce-142">Men om du inte väljer hello bästa kolumnen hello första gången, kan du enkelt nytt distribuera hello data på en annan kolumn.</span><span class="sxs-lookup"><span data-stu-id="99fce-142">However, if you don't choose hello best column hello first time, you can easily re-distribute hello data on a different column.</span></span> 

<span data-ttu-id="99fce-143">toochoose hello bästa distributionsalternativ för varje tabell finns [distribuerade tabeller](sql-data-warehouse-tables-distribute.md).</span><span class="sxs-lookup"><span data-stu-id="99fce-143">toochoose hello best distribution option for each table, see [Distributed tables](sql-data-warehouse-tables-distribute.md).</span></span>


## <a name="next-steps"></a><span data-ttu-id="99fce-144">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99fce-144">Next steps</span></span>
<span data-ttu-id="99fce-145">När du har migrerat dina databasen schemat tooSQL datalagret fortsätta tooone av hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="99fce-145">Once you have successfully migrated your database schema tooSQL Data Warehouse, proceed tooone of hello following articles:</span></span>

* <span data-ttu-id="99fce-146">[Migrera dina data][Migrate your data]</span><span class="sxs-lookup"><span data-stu-id="99fce-146">[Migrate your data][Migrate your data]</span></span>
* <span data-ttu-id="99fce-147">[Migrera koden][Migrate your code]</span><span class="sxs-lookup"><span data-stu-id="99fce-147">[Migrate your code][Migrate your code]</span></span>

<span data-ttu-id="99fce-148">Mer information om metodtips för SQL Data Warehouse finns hello [metodtips] [ best practices] artikel.</span><span class="sxs-lookup"><span data-stu-id="99fce-148">For more about SQL Data Warehouse best practices, see hello [best practices][best practices] article.</span></span>

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
