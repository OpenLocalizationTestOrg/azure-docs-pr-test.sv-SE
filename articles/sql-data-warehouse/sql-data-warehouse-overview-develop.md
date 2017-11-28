---
title: "aaaResources för att utveckla ett data warehouse i Azure | Microsoft Docs"
description: "Begrepp för utveckling, designbeslut, rekommendationer och kodning tekniker för SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: barbkess
editor: 
ms.assetid: 996e3afc-c21c-4e21-b9df-997f953f6dfd
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: develop
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 67e3a6a3e2664919c3445ea5d5eba251054de020
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a><span data-ttu-id="2dfda-103">Designbeslut och kodning tekniker för SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="2dfda-103">Design decisions and coding techniques for SQL Data Warehouse</span></span>
<span data-ttu-id="2dfda-104">Titta igenom dessa development artiklar toobetter förstå viktiga designbeslut, rekommendationer och kodning tekniker för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dfda-104">Take a look through these development articles toobetter understand key design decisions, recommendations and coding techniques for SQL Data Warehouse.</span></span>

## <a name="key-design-decisions"></a><span data-ttu-id="2dfda-105">Viktiga designbeslut</span><span class="sxs-lookup"><span data-stu-id="2dfda-105">Key design decisions</span></span>
<span data-ttu-id="2dfda-106">hello beskrivs följande artiklar några av hello viktiga begrepp och designbeslut som du behöver toounderstand för hello utvecklingen av ditt distribuerade data warehouse med hjälp av SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="2dfda-106">hello following articles highlight some of hello key concepts and design decisions you will need toounderstand for hello development of your distributed data warehouse using SQL Data Warehouse:</span></span>

* <span data-ttu-id="2dfda-107">[anslutningar][connections]</span><span class="sxs-lookup"><span data-stu-id="2dfda-107">[connections][connections]</span></span>
* <span data-ttu-id="2dfda-108">[concurrency][concurrency]</span><span class="sxs-lookup"><span data-stu-id="2dfda-108">[concurrency][concurrency]</span></span>
* <span data-ttu-id="2dfda-109">[transaktioner][transactions]</span><span class="sxs-lookup"><span data-stu-id="2dfda-109">[transactions][transactions]</span></span>
* <span data-ttu-id="2dfda-110">[användardefinierade scheman][user-defined schemas]</span><span class="sxs-lookup"><span data-stu-id="2dfda-110">[user-defined schemas][user-defined schemas]</span></span>
* <span data-ttu-id="2dfda-111">[tabell-distribution][table distribution]</span><span class="sxs-lookup"><span data-stu-id="2dfda-111">[table distribution][table distribution]</span></span>
* <span data-ttu-id="2dfda-112">[Tabellindex][table indexes]</span><span class="sxs-lookup"><span data-stu-id="2dfda-112">[table indexes][table indexes]</span></span>
* <span data-ttu-id="2dfda-113">[tabellpartitioner][table partitions]</span><span class="sxs-lookup"><span data-stu-id="2dfda-113">[table partitions][table partitions]</span></span>
* <span data-ttu-id="2dfda-114">[CTAS][CTAS]</span><span class="sxs-lookup"><span data-stu-id="2dfda-114">[CTAS][CTAS]</span></span>
* <span data-ttu-id="2dfda-115">[statistik][statistics]</span><span class="sxs-lookup"><span data-stu-id="2dfda-115">[statistics][statistics]</span></span>

## <a name="development-recommendations-and-coding-techniques"></a><span data-ttu-id="2dfda-116">Rekommendationer för utveckling och kodning tekniker</span><span class="sxs-lookup"><span data-stu-id="2dfda-116">Development recommendations and coding techniques</span></span>
<span data-ttu-id="2dfda-117">Dessa artiklar markera specifika kodning tekniker, tips och rekommendationer för att utveckla ditt SQL Data Warehouse:</span><span class="sxs-lookup"><span data-stu-id="2dfda-117">These articles highlight specific coding techniques, tips and recommendations for developing your SQL Data Warehouse:</span></span>

* <span data-ttu-id="2dfda-118">[lagrade procedurer][stored procedures]</span><span class="sxs-lookup"><span data-stu-id="2dfda-118">[stored procedures][stored procedures]</span></span>
* <span data-ttu-id="2dfda-119">[etiketter][labels]</span><span class="sxs-lookup"><span data-stu-id="2dfda-119">[labels][labels]</span></span>
* <span data-ttu-id="2dfda-120">[vyer][views]</span><span class="sxs-lookup"><span data-stu-id="2dfda-120">[views][views]</span></span>
* <span data-ttu-id="2dfda-121">[temporära tabeller][temporary tables]</span><span class="sxs-lookup"><span data-stu-id="2dfda-121">[temporary tables][temporary tables]</span></span>
* <span data-ttu-id="2dfda-122">[dynamisk SQL][dynamic SQL]</span><span class="sxs-lookup"><span data-stu-id="2dfda-122">[dynamic SQL][dynamic SQL]</span></span>
* <span data-ttu-id="2dfda-123">[slingor][looping]</span><span class="sxs-lookup"><span data-stu-id="2dfda-123">[looping][looping]</span></span>
* <span data-ttu-id="2dfda-124">[Gruppera efter alternativ][group by options]</span><span class="sxs-lookup"><span data-stu-id="2dfda-124">[group by options][group by options]</span></span>
* <span data-ttu-id="2dfda-125">[variabeltilldelning][variable assignment]</span><span class="sxs-lookup"><span data-stu-id="2dfda-125">[variable assignment][variable assignment]</span></span>

## <a name="next-steps"></a><span data-ttu-id="2dfda-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2dfda-126">Next steps</span></span>
<span data-ttu-id="2dfda-127">När du har gått igenom hello development artiklar ta en titt på hello [Transact-SQL referens] [ Transact-SQL reference] ha mer information om hello stöds syntax för SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="2dfda-127">Once you have been through hello development articles take a look through hello [Transact-SQL reference][Transact-SQL reference] page for more details on hello supported syntax for SQL Data Warehouse.</span></span>

<!--Image references-->

<!--Article references-->
[concurrency]: ./sql-data-warehouse-develop-concurrency.md
[connections]: ./sql-data-warehouse-connect-overview.md
[CTAS]: ./sql-data-warehouse-develop-ctas.md
[dynamic SQL]: ./sql-data-warehouse-develop-dynamic-sql.md
[group by options]: ./sql-data-warehouse-develop-group-by-options.md
[labels]: ./sql-data-warehouse-develop-label.md
[looping]: ./sql-data-warehouse-develop-loops.md
[statistics]: ./sql-data-warehouse-tables-statistics.md
[stored procedures]: ./sql-data-warehouse-develop-stored-procedures.md
[table distribution]: ./sql-data-warehouse-tables-distribute.md
[table indexes]: ./sql-data-warehouse-tables-index.md
[table partitions]: ./sql-data-warehouse-tables-partition.md
[temporary tables]: ./sql-data-warehouse-tables-temporary.md
[transactions]: ./sql-data-warehouse-develop-transactions.md
[user-defined schemas]: ./sql-data-warehouse-develop-user-defined-schemas.md
[variable assignment]: ./sql-data-warehouse-develop-variable-assignment.md
[views]: ./sql-data-warehouse-develop-views.md
[Transact-SQL reference]: ./sql-data-warehouse-overview-reference.md

<!--MSDN references-->
[renaming objects]: https://msdn.microsoft.com/library/mt631611.aspx

<!--Other Web references-->
