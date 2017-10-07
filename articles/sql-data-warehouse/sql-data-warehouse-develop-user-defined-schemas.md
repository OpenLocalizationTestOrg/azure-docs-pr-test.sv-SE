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
# <a name="user-defined-schemas-in-sql-data-warehouse"></a>Användardefinierade scheman i SQL Data Warehouse
Traditionella informationslager ofta använda separata databaser toocreate programmet gränser baserat på arbetsbelastning, domän och säkerhet. En traditionell SQL Server-datalagret kan exempelvis innehålla en fristående databas, en databas för datalager och vissa data mart-databaser. I den här topologin fungerar varje databas som en arbetsbelastning och säkerhetsgräns i hello arkitektur.

Däremot kör SQL Data Warehouse hello hela arbetsbelastningen för informationslager inom en databas. Mellan databasen tillåts inte kopplingar. SQL Data Warehouse förväntar därför alla tabeller som används av hello datalager toobe lagras i en hello-databas.

> [!NOTE]
> SQL Data Warehouse stöder inte mellan databasfrågor av något slag. Därför behöver data warehouse implementeringar som använder det här mönstret toobe omarbetad.
> 
> 

## <a name="recommendations"></a>Rekommendationer
Dessa är rekommendationer för att konsolidera arbetsbelastningar, säkerhet, domän och funktionella gränser med hjälp av användardefinierade scheman

1. Använd en SQL Data Warehouse-databas toorun din hela arbetsbelastningen för informationslager
2. Konsolidera din befintliga data warehouse miljö toouse en SQL Data Warehouse-databas
3. Utnyttjar **användardefinierade scheman** tooprovide hello gräns tidigare implementeras med hjälp av databaser.

Om användardefinierade scheman inte har använts tidigare har du grunden. Helt enkelt använda hello gamla databasnamnet som hello bas för dina egna scheman i hello SQL Data Warehouse-databas.

Om scheman har redan använts har du några alternativ:

1. Ta bort hello äldre schemanamn och börja om från början
2. Behåll hello äldre schemanamn efter före väntande hello äldre namn toohello tabell schemanamn
3. Behåll hello äldre schemanamn genom att implementera vyer över hello-tabellen i en extra schemat toore-skapa hello gamla datastrukturen.

> [!NOTE]
> På första inspektion verka alternativ 3 hello intressanta alternativ. Hej djävulens är dock hello detaljerat. Vyer är skrivskyddad i SQL Data Warehouse. Alla ändringar av data eller tabell måste toobe utföras mot hello bastabellen. Alternativ 3 introducerar också ett lager av vyer i systemet. Du kanske vill toogive detta ytterligare upp om du använder vyer i din arkitektur redan.
> 
> 

### <a name="examples"></a>Exempel:
Implementera anpassade scheman baserat på databasnamn

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

Behålla äldre schemat trots att av före väntande dem toohello tabellnamn. Scheman används för hello arbetsbelastning gräns.

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

Behålla äldre schemanamn med hjälp av vyer

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
> Ändringar i schemat strategi måste en granskning av hello säkerhetsmodell för hello-databasen. I många fall kanske du kan toosimplify hello säkerhetsmodellen genom att tilldela behörigheter på hello schemat nivå. Om mer detaljerade behörigheter som krävs kan du använda databasroller.
> 
> 

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
