---
title: "Användardefinierade scheman i SQL Data Warehouse | Microsoft Docs"
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
ms.openlocfilehash: dfb58956ad6637cf0f50b4c052ab98fb7c26139d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="user-defined-schemas-in-sql-data-warehouse"></a><span data-ttu-id="da516-103">Användardefinierade scheman i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="da516-103">User-defined schemas in SQL Data Warehouse</span></span>
<span data-ttu-id="da516-104">Traditionella informationslager använder ofta separata databaser för att skapa programmet gränser, baserat på arbetsbelastning, domän och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="da516-104">Traditional data warehouses often use separate databases to create application boundaries based on either workload, domain or security.</span></span> <span data-ttu-id="da516-105">En traditionell SQL Server-datalagret kan exempelvis innehålla en fristående databas, en databas för datalager och vissa data mart-databaser.</span><span class="sxs-lookup"><span data-stu-id="da516-105">For example, a traditional SQL Server data warehouse might include a staging database, a data warehouse database, and some data mart databases.</span></span> <span data-ttu-id="da516-106">Varje databas fungerar som en arbetsbelastning och säkerhetsgräns i arkitekturen i den här topologin.</span><span class="sxs-lookup"><span data-stu-id="da516-106">In this topology each database operates as a workload and security boundary in the architecture.</span></span>

<span data-ttu-id="da516-107">Däremot kör SQL Data Warehouse hela arbetsbelastningen för informationslager inom en databas.</span><span class="sxs-lookup"><span data-stu-id="da516-107">By contrast, SQL Data Warehouse runs the entire data warehouse workload within one database.</span></span> <span data-ttu-id="da516-108">Mellan databasen tillåts inte kopplingar.</span><span class="sxs-lookup"><span data-stu-id="da516-108">Cross database joins are not permitted.</span></span> <span data-ttu-id="da516-109">SQL Data Warehouse förväntar därför alla tabeller som används av för datalagret lagras i en databas.</span><span class="sxs-lookup"><span data-stu-id="da516-109">Therefore SQL Data Warehouse expects all tables used by the warehouse to be stored within the one database.</span></span>

> [!NOTE]
> <span data-ttu-id="da516-110">SQL Data Warehouse stöder inte mellan databasfrågor av något slag.</span><span class="sxs-lookup"><span data-stu-id="da516-110">SQL Data Warehouse does not support cross database queries of any kind.</span></span> <span data-ttu-id="da516-111">Därför behöver data warehouse implementeringar som använder det här mönstret ändras.</span><span class="sxs-lookup"><span data-stu-id="da516-111">Consequently, data warehouse implementations that leverage this pattern will need to be revised.</span></span>
> 
> 

## <a name="recommendations"></a><span data-ttu-id="da516-112">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="da516-112">Recommendations</span></span>
<span data-ttu-id="da516-113">Dessa är rekommendationer för att konsolidera arbetsbelastningar, säkerhet, domän och funktionella gränser med hjälp av användardefinierade scheman</span><span class="sxs-lookup"><span data-stu-id="da516-113">These are recommendations for consolidating workloads, security, domain and functional boundaries by using user defined schemas</span></span>

1. <span data-ttu-id="da516-114">Använd en SQL Data Warehouse-databas för att köra din hela arbetsbelastningen för informationslager</span><span class="sxs-lookup"><span data-stu-id="da516-114">Use one SQL Data Warehouse database to run your entire data warehouse workload</span></span>
2. <span data-ttu-id="da516-115">Konsolidera din befintliga data warehouse-miljö för att använda en SQL Data Warehouse-databas</span><span class="sxs-lookup"><span data-stu-id="da516-115">Consolidate your existing data warehouse environment to use one SQL Data Warehouse database</span></span>
3. <span data-ttu-id="da516-116">Utnyttjar **användardefinierade scheman** att ange kolumnrubrikens tidigare implementeras med hjälp av databaser.</span><span class="sxs-lookup"><span data-stu-id="da516-116">Leverage **user-defined schemas** to provide the boundary previously implemented using databases.</span></span>

<span data-ttu-id="da516-117">Om användardefinierade scheman inte har använts tidigare har du grunden.</span><span class="sxs-lookup"><span data-stu-id="da516-117">If user-defined schemas have not been used previously then you have a clean slate.</span></span> <span data-ttu-id="da516-118">Använd bara gamla databasnamnet som bas för dina egna scheman i SQL Data Warehouse-databas.</span><span class="sxs-lookup"><span data-stu-id="da516-118">Simply use the old database name as the basis for your user-defined schemas in the SQL Data Warehouse database.</span></span>

<span data-ttu-id="da516-119">Om scheman har redan använts har du några alternativ:</span><span class="sxs-lookup"><span data-stu-id="da516-119">If schemas have already been used then you have a few options:</span></span>

1. <span data-ttu-id="da516-120">Ta bort de äldra schemanamn och börja om från början</span><span class="sxs-lookup"><span data-stu-id="da516-120">Remove the legacy schema names and start fresh</span></span>
2. <span data-ttu-id="da516-121">Behålla de äldra schemanamn av före väntande äldre schemanamnet till tabellnamnet</span><span class="sxs-lookup"><span data-stu-id="da516-121">Retain the legacy schema names by pre-pending the legacy schema name to the table name</span></span>
3. <span data-ttu-id="da516-122">Behåll det äldre schemat genom att implementera vyer över tabellen i en extra schemat att återskapa gamla datastrukturen.</span><span class="sxs-lookup"><span data-stu-id="da516-122">Retain the legacy schema names by implementing views over the table in an extra schema to re-create the old schema structure.</span></span>

> [!NOTE]
> <span data-ttu-id="da516-123">På första inspektion verka alternativet intressanta alternativ 3.</span><span class="sxs-lookup"><span data-stu-id="da516-123">On first inspection option 3 may seem like the most appealing option.</span></span> <span data-ttu-id="da516-124">Djävulen är dock i informationen.</span><span class="sxs-lookup"><span data-stu-id="da516-124">However, the devil is in the detail.</span></span> <span data-ttu-id="da516-125">Vyer är skrivskyddad i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="da516-125">Views are read only in SQL Data Warehouse.</span></span> <span data-ttu-id="da516-126">Alla ändringar av data- eller behöver utföras mot bastabellen.</span><span class="sxs-lookup"><span data-stu-id="da516-126">Any data or table modification would need to be performed against the base table.</span></span> <span data-ttu-id="da516-127">Alternativ 3 introducerar också ett lager av vyer i systemet.</span><span class="sxs-lookup"><span data-stu-id="da516-127">Option 3 also introduces a layer of views into your system.</span></span> <span data-ttu-id="da516-128">Du kanske vill ge den här ytterligare upp om du använder vyer i din arkitektur redan.</span><span class="sxs-lookup"><span data-stu-id="da516-128">You might want to give this some additional thought if you are using views in your architecture already.</span></span>
> 
> 

### <a name="examples"></a><span data-ttu-id="da516-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="da516-129">Examples:</span></span>
<span data-ttu-id="da516-130">Implementera anpassade scheman baserat på databasnamn</span><span class="sxs-lookup"><span data-stu-id="da516-130">Implement user-defined schemas based on database names</span></span>

```sql
CREATE SCHEMA [stg]; -- stg previously database name for staging database
GO
CREATE SCHEMA [edw]; -- edw previously database name for the data warehouse
GO
CREATE TABLE [stg].[customer] -- create staging tables in the stg schema
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[customer] -- create data warehouse tables in the edw schema
(       CustKey BIGINT NOT NULL
,       ...
);
```

<span data-ttu-id="da516-131">Behålla äldre schemanamn av före väntande dem till tabellens namn.</span><span class="sxs-lookup"><span data-stu-id="da516-131">Retain legacy schema names by pre-pending them to the table name.</span></span> <span data-ttu-id="da516-132">Scheman används för arbetsbelastningen gräns.</span><span class="sxs-lookup"><span data-stu-id="da516-132">Use schemas for the workload boundary.</span></span>

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- edw defines the data warehouse boundary
GO
CREATE TABLE [stg].[dim_customer] --pre-pend the old schema name to the table and create in the staging boundary
(       CustKey BIGINT NOT NULL
,       ...
);
GO
CREATE TABLE [edw].[dim_customer] --pre-pend the old schema name to the table and create in the data warehouse boundary
(       CustKey BIGINT NOT NULL
,       ...
);
```

<span data-ttu-id="da516-133">Behålla äldre schemanamn med hjälp av vyer</span><span class="sxs-lookup"><span data-stu-id="da516-133">Retain legacy schema names using views</span></span>

```sql
CREATE SCHEMA [stg]; -- stg defines the staging boundary
GO
CREATE SCHEMA [edw]; -- stg defines the data warehouse boundary
GO
CREATE SCHEMA [dim]; -- edw defines the legacy schema name boundary
GO
CREATE TABLE [stg].[customer] -- create the base staging tables in the staging boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE TABLE [edw].[customer] -- create the base data warehouse tables in the data warehouse boundary
(       CustKey    BIGINT NOT NULL
,       ...
)
GO
CREATE VIEW [dim].[customer] -- create a view in the legacy schema name boundary for presentation consistency purposes only
AS
SELECT  CustKey
,       ...
FROM    [edw].customer
;
```

> [!NOTE]
> <span data-ttu-id="da516-134">Ändringar i schemat strategi måste en granskning av säkerhetsmodellen för databasen.</span><span class="sxs-lookup"><span data-stu-id="da516-134">Any change in schema strategy needs a review of the security model for the database.</span></span> <span data-ttu-id="da516-135">I många fall kanske du kan förenkla säkerhetsmodellen genom att tilldela behörigheter på schemanivån.</span><span class="sxs-lookup"><span data-stu-id="da516-135">In many cases you might be able to simplify the security model by assigning permissions at the schema level.</span></span> <span data-ttu-id="da516-136">Om mer detaljerade behörigheter som krävs kan du använda databasroller.</span><span class="sxs-lookup"><span data-stu-id="da516-136">If more granular permissions are required then you can use database roles.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="da516-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="da516-137">Next steps</span></span>
<span data-ttu-id="da516-138">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="da516-138">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
