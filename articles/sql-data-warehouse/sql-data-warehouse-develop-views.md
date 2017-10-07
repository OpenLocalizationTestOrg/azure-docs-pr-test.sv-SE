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
# <a name="views-in-sql-data-warehouse"></a>Vyer i SQL Data Warehouse
Vyer är särskilt användbart i SQL Data Warehouse. De kan användas i ett antal olika sätt tooimprove hello kvaliteten på din lösning.  Den här artikeln visar några exempel på hur tooenrich din lösning med vyer, samt hello begränsningar som behöver toobe anses vara.

> [!NOTE]
> Syntaxen för `CREATE VIEW` beskrivs inte i den här artikeln. Se toohello [skapa vy] [ CREATE VIEW] -artikel på MSDN den här referensen.
> 
> 

## <a name="architectural-abstraction"></a>Arkitektur abstraction
Ett mycket vanligt mönstret för programmet är toore-skapa tabeller med Skapa tabell AS Välj (CTAS) följt av ett objekt som ett mönster medan data läses in.

hello exemplet nedan lägger till nya poster tooa datum datumdimension. Observera hur en ny tabble DimDate_New, skapas och sedan ett nytt namn tooreplace hello originalversionen av hello tabell.

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

Den här metoden kan emellertid resultera i tabeller som visas och försvinner från vyn för en användare som ”tabellen finns inte” felmeddelanden. Vyer kan vara används tooprovide användare med ett konsekvent presentation lager samtidigt hello underliggande objekt ändras. Genom att ge användare åtkomst innebär toohello data via en vyer, att användarna inte behöver toohave synlighet för hello underliggande tabeller. Detta ger en konsekvent användarupplevelse vid säkerställer att hello data warehouse Designer kan utvecklas hello-datamodell och maximera prestanda med hjälp av CTAS under hello datainläsning processen.    

## <a name="performance-optimization"></a>Prestandaoptimering
Vyer kan också vara utnyttjade tooenforce prestanda optimerade kopplingar mellan tabeller. En vy kan exempelvis innehålla en redundant distributionsnyckeln som en del av hello koppla kriterier toominimize dataflyttning.  En annan fördel med en vy kan vara tooforce en specifik fråga eller anslutande tipset. Använda vyer i det här sättet garanterar att kopplingar utförs alltid på ett optimalt sätt att undvika hello behovet av att användare tooremember hello rätt konstruktion för deras kopplingar.

## <a name="limitations"></a>Begränsningar
Vyer i SQL Data Warehouse är endast metadata.  Därför är hello följande alternativ inte tillgängliga:

* Det finns inget alternativ för bindning av schemat
* Bastabeller kan inte uppdateras via hello vy
* Att går inte skapa vyer över temporära tabeller
* Det finns inget stöd för hello Expandera / NOEXPAND-tips
* Det finns inga indexerade vyer i SQL Data Warehouse

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [Översikt över SQL Data Warehouse-utveckling][SQL Data Warehouse development overview].
För `CREATE VIEW` syntax finns för[skapa vy][CREATE VIEW].

<!--Image references-->

<!--Article references-->
[SQL Data Warehouse development overview]: ./sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[CREATE VIEW]: https://msdn.microsoft.com/en-us/library/ms187956.aspx

<!--Other Web references-->
