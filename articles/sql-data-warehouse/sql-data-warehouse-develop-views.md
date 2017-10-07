---
title: aaaUsing T-SQL-vyer i Azure SQL Data Warehouse | Microsoft Docs
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
ms.openlocfilehash: 3990b133946621691bdfa4b09523d21867470c74
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="views-in-sql-data-warehouse"></a><span data-ttu-id="81a70-103">Vyer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="81a70-103">Views in SQL Data Warehouse</span></span>
<span data-ttu-id="81a70-104">Vyer är särskilt användbart i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="81a70-104">Views are particularly useful in SQL Data Warehouse.</span></span> <span data-ttu-id="81a70-105">De kan användas i ett antal olika sätt tooimprove hello kvaliteten på din lösning.</span><span class="sxs-lookup"><span data-stu-id="81a70-105">They can be used in a number of different ways tooimprove hello quality of your solution.</span></span>  <span data-ttu-id="81a70-106">Den här artikeln visar några exempel på hur tooenrich din lösning med vyer, samt hello begränsningar som behöver toobe anses vara.</span><span class="sxs-lookup"><span data-stu-id="81a70-106">This article highlights a few examples of how tooenrich your solution with views, as well as hello limitations that need toobe considered.</span></span>

> [!NOTE]
> <span data-ttu-id="81a70-107">Syntaxen för `CREATE VIEW` beskrivs inte i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="81a70-107">Syntax for `CREATE VIEW` is not discussed in this article.</span></span> <span data-ttu-id="81a70-108">Se toohello [skapa vy] [ CREATE VIEW] -artikel på MSDN den här referensen.</span><span class="sxs-lookup"><span data-stu-id="81a70-108">Please refer toohello [CREATE VIEW][CREATE VIEW] article on MSDN for this reference information.</span></span>
> 
> 

## <a name="architectural-abstraction"></a><span data-ttu-id="81a70-109">Arkitektur abstraction</span><span class="sxs-lookup"><span data-stu-id="81a70-109">Architectural abstraction</span></span>
<span data-ttu-id="81a70-110">Ett mycket vanligt mönstret för programmet är toore-skapa tabeller med Skapa tabell AS Välj (CTAS) följt av ett objekt som ett mönster medan data läses in.</span><span class="sxs-lookup"><span data-stu-id="81a70-110">A very common application pattern is toore-create tables using CREATE TABLE AS SELECT (CTAS) followed by an object renaming pattern whilst loading data.</span></span>

<span data-ttu-id="81a70-111">hello exemplet nedan lägger till nya poster tooa datum datumdimension.</span><span class="sxs-lookup"><span data-stu-id="81a70-111">hello example below adds new date records tooa date dimension.</span></span> <span data-ttu-id="81a70-112">Observera hur en ny tabble DimDate_New, skapas och sedan ett nytt namn tooreplace hello originalversionen av hello tabell.</span><span class="sxs-lookup"><span data-stu-id="81a70-112">Note how a new tabble, DimDate_New, is first created and then renamed tooreplace hello original version of hello table.</span></span>

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

RENAME OBJECT DimDate tooDimDate_Old;
RENAME OBJECT DimDate_New tooDimDate;

```

<span data-ttu-id="81a70-113">Den här metoden kan emellertid resultera i tabeller som visas och försvinner från vyn för en användare som ”tabellen finns inte” felmeddelanden.</span><span class="sxs-lookup"><span data-stu-id="81a70-113">However, this approach can result in tables appearing and disappearing from a user's view as well as "table does not exist" error messages.</span></span> <span data-ttu-id="81a70-114">Vyer kan vara används tooprovide användare med ett konsekvent presentation lager samtidigt hello underliggande objekt ändras.</span><span class="sxs-lookup"><span data-stu-id="81a70-114">Views can be used tooprovide users with a consistent presentation layer whilst hello underlying objects are renamed.</span></span> <span data-ttu-id="81a70-115">Genom att ge användare åtkomst innebär toohello data via en vyer, att användarna inte behöver toohave synlighet för hello underliggande tabeller.</span><span class="sxs-lookup"><span data-stu-id="81a70-115">By providing users access toohello data through a views, means users don't need toohave visibility of hello underlying tables.</span></span> <span data-ttu-id="81a70-116">Detta ger en konsekvent användarupplevelse vid säkerställer att hello data warehouse Designer kan utvecklas hello-datamodell och maximera prestanda med hjälp av CTAS under hello datainläsning processen.</span><span class="sxs-lookup"><span data-stu-id="81a70-116">This provides a consistent user experience while ensuring that hello data warehouse designers can evolve hello data model and maximize performance by using CTAS during hello data loading process.</span></span>    

## <a name="performance-optimization"></a><span data-ttu-id="81a70-117">Prestandaoptimering</span><span class="sxs-lookup"><span data-stu-id="81a70-117">Performance optimization</span></span>
<span data-ttu-id="81a70-118">Vyer kan också vara utnyttjade tooenforce prestanda optimerade kopplingar mellan tabeller.</span><span class="sxs-lookup"><span data-stu-id="81a70-118">Views can also be utilized tooenforce performance optimized joins between tables.</span></span> <span data-ttu-id="81a70-119">En vy kan exempelvis innehålla en redundant distributionsnyckeln som en del av hello koppla kriterier toominimize dataflyttning.</span><span class="sxs-lookup"><span data-stu-id="81a70-119">For example, a view can incorporate a redundant distribution key as part of hello joining criteria toominimize data movement.</span></span>  <span data-ttu-id="81a70-120">En annan fördel med en vy kan vara tooforce en specifik fråga eller anslutande tipset.</span><span class="sxs-lookup"><span data-stu-id="81a70-120">Another benefit of a view could be tooforce a specific query or joining hint.</span></span> <span data-ttu-id="81a70-121">Använda vyer i det här sättet garanterar att kopplingar utförs alltid på ett optimalt sätt att undvika hello behovet av att användare tooremember hello rätt konstruktion för deras kopplingar.</span><span class="sxs-lookup"><span data-stu-id="81a70-121">Using views in this manner guarantees that joins are always performed in an optimal fashion avoiding hello need for users tooremember hello correct construct for their joins.</span></span>

## <a name="limitations"></a><span data-ttu-id="81a70-122">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="81a70-122">Limitations</span></span>
<span data-ttu-id="81a70-123">Vyer i SQL Data Warehouse är endast metadata.</span><span class="sxs-lookup"><span data-stu-id="81a70-123">Views in SQL Data Warehouse are metadata only.</span></span>  <span data-ttu-id="81a70-124">Därför är hello följande alternativ inte tillgängliga:</span><span class="sxs-lookup"><span data-stu-id="81a70-124">Consequently hello following options are not available:</span></span>

* <span data-ttu-id="81a70-125">Det finns inget alternativ för bindning av schemat</span><span class="sxs-lookup"><span data-stu-id="81a70-125">There is no schema binding option</span></span>
* <span data-ttu-id="81a70-126">Bastabeller kan inte uppdateras via hello vy</span><span class="sxs-lookup"><span data-stu-id="81a70-126">Base tables cannot be updated through hello view</span></span>
* <span data-ttu-id="81a70-127">Att går inte skapa vyer över temporära tabeller</span><span class="sxs-lookup"><span data-stu-id="81a70-127">Views cannot be created over temporary tables</span></span>
* <span data-ttu-id="81a70-128">Det finns inget stöd för hello Expandera / NOEXPAND-tips</span><span class="sxs-lookup"><span data-stu-id="81a70-128">There is no support for hello EXPAND / NOEXPAND hints</span></span>
* <span data-ttu-id="81a70-129">Det finns inga indexerade vyer i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="81a70-129">There are no indexed views in SQL Data Warehouse</span></span>

## <a name="next-steps"></a><span data-ttu-id="81a70-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81a70-130">Next steps</span></span>
<span data-ttu-id="81a70-131">För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].</span><span class="sxs-lookup"><span data-stu-id="81a70-131">For more development tips, see [SQL Data Warehouse development overview][SQL Data Warehouse development overview].</span></span>
<span data-ttu-id="81a70-132">För `CREATE VIEW` syntax finns för[skapa vy][CREATE VIEW].</span><span class="sxs-lookup"><span data-stu-id="81a70-132">For `CREATE VIEW` syntax please refer too[CREATE VIEW][CREATE VIEW].</span></span>

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
