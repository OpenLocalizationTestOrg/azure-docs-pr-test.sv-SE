---
title: aaaUser-definierade scheman i SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda Transact-SQL-scheman i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 52af5bd5-d5d3-4f9b-8704-06829fb924e3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: c411d6fed68e67c444a5871eab06182eaeb6dbf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a><span data-ttu-id="367d4-103">Användardefinierade scheman i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="367d4-103">User-defined schemas in SQL Data Warehouse</span></span>
<span data-ttu-id="367d4-104">Traditionella informationslager ofta använda separata databaser toocreate programmet gränser baserat på arbetsbelastning, domän och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="367d4-104">Traditional data warehouses often use separate databases toocreate application boundaries based on either workload, domain or security.</span></span> <span data-ttu-id="367d4-105">En traditionell SQL Server-datalagret kan exempelvis innehålla en fristående databas, en databas för datalager och vissa data mart-databaser.</span><span class="sxs-lookup"><span data-stu-id="367d4-105">For example, a traditional SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="367d4-106">I den här topologin fungerar varje databas som en arbetsbelastning och säkerhetsgräns i hello arkitektur.</span><span class="sxs-lookup"><span data-stu-id="367d4-106">In this topology each database operates as a workload and security boundary in hello architecture.</span></span>

<span data-ttu-id="367d4-107">Däremot kör SQL Data Warehouse hello hela arbetsbelastningen för informationslager inom en databas.</span><span class="sxs-lookup"><span data-stu-id="367d4-107">By contrast, SQL Data Warehouse runs hello entire data warehouse workload within one database.</span></span> <span data-ttu-id="367d4-108">Mellan databasen tillåts inte kopplingar.</span><span class="sxs-lookup"><span data-stu-id="367d4-108">Cross database joins are not permitted.</span></span> <span data-ttu-id="367d4-109">SQL Data Warehouse förväntar därför alla tabeller som används av hello datalager toobe lagras i en hello-databas.</span><span class="sxs-lookup"><span data-stu-id="367d4-109">Therefore SQL Data Warehouse expects all tables used by hello warehouse toobe stored within hello one database.</span></span>

> [!NOTE]
> <span data-ttu-id="367d4-110">SQL Data Warehouse stöder inte mellan databasfrågor av något slag.</span><span class="sxs-lookup"><span data-stu-id="367d4-110">SQL Data Warehouse does not support cross database queries of any kind.</span></span> <span data-ttu-id="367d4-111">Därför behöver data warehouse implementeringar som använder det här mönstret toobe omarbetad.</span><span class="sxs-lookup"><span data-stu-id="367d4-111">Consequently, data warehouse implementations that leverage this pattern will need toobe revised.</span></span>
> 
> 

## <a name="recommendations"></a><span data-ttu-id="367d4-112">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="367d4-112">Recommendations</span></span>
<span data-ttu-id="367d4-113">Dessa är rekommendationer för att konsolidera arbetsbelastningar, säkerhet, domän och funktionella gränser med hjälp av användardefinierade scheman</span><span class="sxs-lookup"><span data-stu-id="367d4-113">These are recommendations for consolidating workloads, security, domain and functional boundaries by using user defined schemas</span></span>

1. <span data-ttu-id="367d4-114">Använd en SQL Data Warehouse-databas toorun din hela arbetsbelastningen för informationslager</span><span class="sxs-lookup"><span data-stu-id="367d4-114">Use one SQL Data Warehouse database toorun your entire data warehouse workload</span></span>
2. <span data-ttu-id="367d4-115">Konsolidera din befintliga data warehouse miljö toouse en SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="367d4-115">Consolidate your existing data warehouse environment toouse one SQL Data Warehouse database</span></span>
3. <span data-ttu-id="367d4-116">Utnyttjar **användardefinierade scheman** tooprovide hello gräns tidigare implementeras med hjälp av databaser.</span><span class="sxs-lookup"><span data-stu-id="367d4-116">Leverage **user-defined schemas** tooprovide hello boundary previously implemented using databases.</span></span>

<span data-ttu-id="367d4-117">Om användardefinierade scheman inte har använts tidigare har du grunden.</span><span class="sxs-lookup"><span data-stu-id="367d4-117">If user-defined schemas have not been used previously then you have a clean slate.</span></span> <span data-ttu-id="367d4-118">Helt enkelt använda hello gamla databasnamnet som hello bas för dina egna scheman i hello SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="367d4-118">Simply use hello old database name as hello basis for your user-defined schemas in hello SQL Data Warehouse database.</span></span>

<span data-ttu-id="367d4-119">Om scheman har redan använts har du några alternativ:</span><span class="sxs-lookup"><span data-stu-id="367d4-119">If schemas have already been used then you have a few options:</span></span>

1. <span data-ttu-id="367d4-120">Ta bort hello äldre schemanamn och börja om från början</span><span class="sxs-lookup"><span data-stu-id="367d4-120">Remove hello legacy schema names and start fresh</span></span>
2. <span data-ttu-id="367d4-121">Behåll hello äldre schemanamn efter före väntande hello äldre namn toohello tabell schemanamn</span><span class="sxs-lookup"><span data-stu-id="367d4-121">Retain hello legacy schema names by pre-pending hello legacy schema name toohello table name</span></span>
3. <span data-ttu-id="367d4-122">Behåll hello äldre schemanamn genom att implementera vyer över hello-tabellen i en extra schemat toore-skapa hello gamla datastrukturen.</span><span class="sxs-lookup"><span data-stu-id="367d4-122">Retain hello legacy schema names by implementing views over hello table in an extra schema toore-create hello old schema structure.</span></span>

> [!NOTE]
> <span data-ttu-id="367d4-123">På första inspektion verka alternativ 3 hello intressanta alternativ.</span><span class="sxs-lookup"><span data-stu-id="367d4-123">On first inspection option 3 may seem like hello most appealing option.</span></span> <span data-ttu-id="367d4-124">Hej djävulens är dock hello detaljerat.</span><span class="sxs-lookup"><span data-stu-id="367d4-124">However, hello devil is in hello detail.</span></span> <span data-ttu-id="367d4-125">Vyer är skrivskyddad i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="367d4-125">Views are read only in SQL Data Warehouse.</span></span> <span data-ttu-id="367d4-126">Alla ändringar av data eller tabell måste toobe utföras mot hello bastabellen.</span><span class="sxs-lookup"><span data-stu-id="367d4-126">Any data or table modification would need toobe performed against hello base table.</span></span> <span data-ttu-id="367d4-127">Alternativ 3 introducerar också ett lager av vyer i systemet.</span><span class="sxs-lookup"><span data-stu-id="367d4-127">Option 3 also introduces a layer of views into your system.</span></span> <span data-ttu-id="367d4-128">Du kanske vill toogive detta ytterligare upp om du använder vyer i din arkitektur redan.</span><span class="sxs-lookup"><span data-stu-id="367d4-128">You might want toogive this some additional thought if you are using views in your architecture already.</span></span>
> 
> 

### <a name="examples"></a><span data-ttu-id="367d4-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="367d4-129">Examples:</span></span>
<span data-ttu-id="367d4-130">Implementera anpassade scheman baserat på databasnamn</span><span class="sxs-lookup"><span data-stu-id="367d4-130">Implement user-defined schemas based on database names</span></span>

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for hello data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in hello stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in hello edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

<span data-ttu-id="367d4-131">Behålla äldre schemat trots att av före väntande dem toohello tabellnamn.</span><span class="sxs-lookup"><span data-stu-id="367d4-131">Retain legacy schema names by pre-pending them toohello table name.</span></span> <span data-ttu-id="367d4-132">Scheman används för hello arbetsbelastning gräns.</span><span class="sxs-lookup"><span data-stu-id="367d4-132">Use schemas for hello workload boundary.</span></span>

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines hello data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend hello old schema name toohello table and create in hello staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend hello old schema name toohello table and create in hello data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

<span data-ttu-id="367d4-133">Behålla äldre schemanamn med hjälp av vyer</span><span class="sxs-lookup"><span data-stu-id="367d4-133">Retain legacy schema names using views</span></span>

```sql
CREATE SCHEMA [stg]; -- stg defines hello staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines hello data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines hello legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create hello base staging tables in hello staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create hello base data warehouse tables in hello data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in hello legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> <span data-ttu-id="367d4-134">Ändringar i schemat strategi måste en granskning av hello säkerhetsmodell för hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="367d4-134">Any change in schema strategy needs a review of hello security model for hello database.</span></span> <span data-ttu-id="367d4-135">I många fall kanske du kan toosimplify hello säkerhetsmodellen genom att tilldela behörigheter på hello schemat nivå.</span><span class="sxs-lookup"><span data-stu-id="367d4-135">In many cases you might be able toosimplify hello security model by assigning permissions at hello schema level.</span></span> <span data-ttu-id="367d4-136">Om mer detaljerade behörigheter som krävs kan du använda databasroller.</span><span class="sxs-lookup"><span data-stu-id="367d4-136">If more granular permissions are required then you can use database roles.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="367d4-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="367d4-137">Next steps</span></span>
<span data-ttu-id="367d4-138">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="367d4-138">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
