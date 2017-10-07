---
title: aaaDynamic SQL i SQL Data Warehouse | Microsoft Docs
description: "Tips för att använda dynamisk SQL i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: a948c2c3-3cd1-4373-90a9-79e59414b778
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 4d66eecb37621510f657d1ec9a2a935daaa16052
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-sql-in-sql-data-warehouse"></a>Dynamisk SQL i SQL Data Warehouse
När du utvecklar programkod för SQL Data Warehouse som du kanske måste toouse dynamisk sql toohelp levererar flexibla, allmän och modulär lösningar. SQL Data Warehouse stöder inte blob-datatyper just nu. Detta kan begränsa hello storleken på din strängar som blob-typer innehålla både varchar(max) och nvarchar(max) typer. Om du har använt dessa typer i din programkod när du skapar mycket stora strängar, behöver du toobreak hello koden i segment och Använd hello EXEC-instruktion i stället.

Ett enkelt exempel:

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

Om hello sträng är kort kan du använda [sp_executesql] [ sp_executesql] som vanligt.

> [!NOTE]
> Instruktioner som körs som dynamiska SQL kommer fortfarande att ämne tooall TSQL valideringsregler.
> 
> 

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
