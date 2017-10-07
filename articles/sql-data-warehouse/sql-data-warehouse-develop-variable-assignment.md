---
title: aaaAssign variabler i SQL Data Warehouse | Microsoft Docs
description: "Tips för att tilldela Transact-SQL-variabler i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 81ddc7cf-a6ba-4585-91a3-b6ea50f49227
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: t-sql
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 9de48739bb0af80ff2a117704b31512c680f78d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="assign-variables-in-sql-data-warehouse"></a>Tilldela variabler i SQL Data Warehouse
Variabler i SQL Data Warehouse anges med hjälp av hello `DECLARE` instruktion eller hello `SET` instruktionen.

Alla följande hello är perfekt ogiltig sätt tooset ett variabelvärde:

## <a name="setting-variables-with-declare"></a>Ange variabler med DECLARE
Initiera variabler med DECLARE är en av hello mest flexibla sätt tooset variabelvärdet i SQL Data Warehouse.

```sql
DECLARE @v  int = 0
;
```

Du kan också använda DECLARE tooset mer än en variabel i taget. Du kan inte använda `SELECT` eller `UPDATE` toodo detta:

```sql
DECLARE @v  INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Smith')
,       @v1 INT = (SELECT TOP 1 c_customer_sk FROM Customer where c_last_name = 'Jones')
;
```

Du kan inte initiera och använda en variabel i hello samma DECLARE-sats. tooillustrate hello punkt hello exemplet nedan har **inte** tillåtna som @p1 är både initierats och användas i hello samma DECLARE-sats. Detta resulterar i ett fel.

```sql
DECLARE @p1 int = 0
,       @p2 int = (SELECT COUNT (*) FROM sys.types where is_user_defined = @p1 )
;
```

## <a name="setting-values-with-set"></a>Ange värden med
Är en mycket vanlig metod för att ställa in en enskild variabel.

Alla hello exemplen nedan är giltiga sätt för en variabel med:

```sql
SET     @v = (Select max(database_id) from sys.databases);
SET     @v = 1;
SET     @v = @v+1;
SET     @v +=1;
```

Du kan bara ange en variabel i taget med. Men som kan ses över sammansatt operatorer är tillåtna.

## <a name="limitations"></a>Begränsningar
Du kan inte använda SELECT- eller UPDATE för variabeltilldelning.

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
