---
title: "Använda T-SQL-vyer i Azure SQL Data Warehouse | Microsoft Docs"
description: "Tips för att använda Transact-SQL-vyer i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: b5208f32-8f4a-4056-8788-2adbb253d9fd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: d2a03be810bd7f792876607ec735eb578b65a3b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="views-in-sql-data-warehouse"></a><span data-ttu-id="a885f-103">Vyer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="a885f-103">Views in SQL Data Warehouse</span></span>
<span data-ttu-id="a885f-104">Vyer är särskilt användbart i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a885f-104">Views are particularly useful in SQL Data Warehouse.</span></span> <span data-ttu-id="a885f-105">De kan användas i ett antal olika sätt att förbättra kvaliteten på din lösning.</span><span class="sxs-lookup"><span data-stu-id="a885f-105">They can be used in a number of different ways to improve the quality of your solution.</span></span>  <span data-ttu-id="a885f-106">Den här artikeln visar några exempel på hur du utöka din lösning med vyer, samt begränsningarna som måste beaktas.</span><span class="sxs-lookup"><span data-stu-id="a885f-106">This article highlights a few examples of how to enrich your solution with views, as well as the limitations that need to be considered.</span></span>

> [!NOTE]
> <span data-ttu-id="a885f-107">Syntaxen för `CREATE VIEW` beskrivs inte i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="a885f-107">Syntax for `CREATE VIEW` is not discussed in this article.</span></span> <span data-ttu-id="a885f-108">Mer information finns i [skapa vy] [ CREATE VIEW] -artikel på MSDN denna referens.</span><span class="sxs-lookup"><span data-stu-id="a885f-108">Please refer to the [CREATE VIEW][CREATE VIEW] article on MSDN for this reference information.</span></span>
> 
> 

## <a name="architectural-abstraction"></a><span data-ttu-id="a885f-109">Arkitektur abstraction</span><span class="sxs-lookup"><span data-stu-id="a885f-109">Architectural abstraction</span></span>
<span data-ttu-id="a885f-110">Ett mycket vanligt mönster för programmet är att skapa tabeller med Skapa tabell AS Välj (CTAS) följt av ett objekt som ett mönster medan data läses in igen.</span><span class="sxs-lookup"><span data-stu-id="a885f-110">A very common application pattern is to re-create tables using CREATE TABLE AS SELECT (CTAS) followed by an object renaming pattern whilst loading data.</span></span>

<span data-ttu-id="a885f-111">Exemplet nedan lägger till nya datum poster till en datumdimension.</span><span class="sxs-lookup"><span data-stu-id="a885f-111">The example below adds new date records to a date dimension.</span></span> <span data-ttu-id="a885f-112">Observera hur en ny tabble DimDate_New, skapas först och sedan ett nytt namn om du vill ersätta den ursprungliga versionen av tabellen.</span><span class="sxs-lookup"><span data-stu-id="a885f-112">Note how a new tabble, DimDate_New, is first created and then renamed to replace the original version of the table.</span></span>

```sql
CREATE TABLE dbo.DimDate_New
WITH (DISTRIBUTION = ROUND_ROBIN
, CLUSTERED INDEX (DateKey ASC)
)
AS
SELECT *
FROM   dbo.DimDate  AS prod
UNION ALL
SELECT *
FROM   dbo.DimDate_stg AS stg
;

RENAME OBJECT DimDate TO DimDate_Old;
RENAME OBJECT DimDate_New TO DimDate;

```

<span data-ttu-id="a885f-113">Den här metoden kan emellertid resultera i tabeller som visas och försvinner från vyn för en användare som ”tabellen finns inte” felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="a885f-113">However, this approach can result in tables appearing and disappearing from a user's view as well as "table does not exist" error messages.</span></span> <span data-ttu-id="a885f-114">Vyer kan användas för att ge användarna en konsekvent presentation layer samtidigt som de underliggande objekt ändras.</span><span class="sxs-lookup"><span data-stu-id="a885f-114">Views can be used to provide users with a consistent presentation layer whilst the underlying objects are renamed.</span></span> <span data-ttu-id="a885f-115">Genom att ge användare åtkomst till data via en vyer, innebär att användarna inte behöver ha synlighet för de underliggande tabellerna.</span><span class="sxs-lookup"><span data-stu-id="a885f-115">By providing users access to the data through a views, means users don't need to have visibility of the underlying tables.</span></span> <span data-ttu-id="a885f-116">Detta ger en konsekvent användarupplevelse medan säkerställer att datalagret designers kan utvecklas datamodellen och maximera prestanda med hjälp av CTAS under processen för datainläsning.</span><span class="sxs-lookup"><span data-stu-id="a885f-116">This provides a consistent user experience while ensuring that the data warehouse designers can evolve the data model and maximize performance by using CTAS during the data loading process.</span></span>    

## <a name="performance-optimization"></a><span data-ttu-id="a885f-117">Prestandaoptimering</span><span class="sxs-lookup"><span data-stu-id="a885f-117">Performance optimization</span></span>
<span data-ttu-id="a885f-118">Vyer kan också användas för att genomdriva prestanda optimerade kopplingar mellan tabeller.</span><span class="sxs-lookup"><span data-stu-id="a885f-118">Views can also be utilized to enforce performance optimized joins between tables.</span></span> <span data-ttu-id="a885f-119">En vy kan inkludera en redundant distribution tangent som en del av anslutande villkoren för att minimera dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="a885f-119">For example, a view can incorporate a redundant distribution key as part of the joining criteria to minimize data movement.</span></span>  <span data-ttu-id="a885f-120">En annan fördel med en vy kan vara att tvinga en specifik fråga eller anslutande tipset.</span><span class="sxs-lookup"><span data-stu-id="a885f-120">Another benefit of a view could be to force a specific query or joining hint.</span></span> <span data-ttu-id="a885f-121">Använda vyer i det här sättet garanterar att kopplingar utförs alltid på ett optimalt sätt att undvika behovet för användare att komma ihåg rätt konstruktion för deras kopplingar.</span><span class="sxs-lookup"><span data-stu-id="a885f-121">Using views in this manner guarantees that joins are always performed in an optimal fashion avoiding the need for users to remember the correct construct for their joins.</span></span>

## <a name="limitations"></a><span data-ttu-id="a885f-122">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="a885f-122">Limitations</span></span>
<span data-ttu-id="a885f-123">Vyer i SQL Data Warehouse är endast metadata.</span><span class="sxs-lookup"><span data-stu-id="a885f-123">Views in SQL Data Warehouse are metadata only.</span></span>  <span data-ttu-id="a885f-124">Följande alternativ är därför inte tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="a885f-124">Consequently the following options are not available:</span></span>

* <span data-ttu-id="a885f-125">Det finns inget alternativ för bindning av schemat</span><span class="sxs-lookup"><span data-stu-id="a885f-125">There is no schema binding option</span></span>
* <span data-ttu-id="a885f-126">Bastabeller kan inte uppdateras via vyn</span><span class="sxs-lookup"><span data-stu-id="a885f-126">Base tables cannot be updated through the view</span></span>
* <span data-ttu-id="a885f-127">Att går inte skapa vyer över temporära tabeller</span><span class="sxs-lookup"><span data-stu-id="a885f-127">Views cannot be created over temporary tables</span></span>
* <span data-ttu-id="a885f-128">Det finns inget stöd för Expandera / NOEXPAND-tips</span><span class="sxs-lookup"><span data-stu-id="a885f-128">There is no support for the EXPAND / NOEXPAND hints</span></span>
* <span data-ttu-id="a885f-129">Det finns inga indexerade vyer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="a885f-129">There are no indexed views in SQL Data Warehouse</span></span>

## <a name="next-steps"></a><span data-ttu-id="a885f-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a885f-130">Next steps</span></span>
<span data-ttu-id="a885f-131">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="a885f-131">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="a885f-132">För `CREATE VIEW` syntax Se [skapa vy][CREATE VIEW].</span><span class="sxs-lookup"><span data-stu-id="a885f-132">For `CREATE VIEW` syntax please refer to [CREATE VIEW][CREATE VIEW].</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
