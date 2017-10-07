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
# <a name="design-decisions-and-coding-techniques-for-sql-data-warehouse"></a>Designbeslut och kodning tekniker för SQL Data Warehouse
Titta igenom dessa development artiklar toobetter förstå viktiga designbeslut, rekommendationer och kodning tekniker för SQL Data Warehouse.

## <a name="key-design-decisions"></a>Viktiga designbeslut
hello beskrivs följande artiklar några av hello viktiga begrepp och designbeslut som du behöver toounderstand för hello utvecklingen av ditt distribuerade data warehouse med hjälp av SQL Data Warehouse:

* [anslutningar][connections]
* [concurrency][concurrency]
* [transaktioner][transactions]
* [användardefinierade scheman][user-defined schemas]
* [tabell-distribution][table distribution]
* [Tabellindex][table indexes]
* [tabellpartitioner][table partitions]
* [CTAS][CTAS]
* [statistik][statistics]

## <a name="development-recommendations-and-coding-techniques"></a>Rekommendationer för utveckling och kodning tekniker
Dessa artiklar markera specifika kodning tekniker, tips och rekommendationer för att utveckla ditt SQL Data Warehouse:

* [lagrade procedurer][stored procedures]
* [etiketter][labels]
* [vyer][views]
* [temporära tabeller][temporary tables]
* [dynamisk SQL][dynamic SQL]
* [slingor][looping]
* [Gruppera efter alternativ][group by options]
* [variabeltilldelning][variable assignment]

## <a name="next-steps"></a>Nästa steg
När du har gått igenom hello development artiklar ta en titt på hello [Transact-SQL referens] [ Transact-SQL reference] ha mer information om hello stöds syntax för SQL Data Warehouse.

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
