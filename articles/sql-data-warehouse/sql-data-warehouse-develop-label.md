---
title: "aaaUse märker tooinstrument frågor i SQL Data Warehouse | Microsoft Docs"
description: "Tips för att använda etiketter tooinstrument frågor i Azure SQL Data Warehouse för utveckling av lösningar."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: jhubbard
editor: 
ms.assetid: 44988de8-04c1-4fed-92be-e1935661a4e8
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: queries
ms.date: 10/31/2016
ms.author: jrj;barbkess
ms.openlocfilehash: 82e7ea98e1417134227f1d7c529fdaf2f1df3853
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-labels-tooinstrument-queries-in-sql-data-warehouse"></a>Använda etiketter tooinstrument frågor i SQL Data Warehouse
SQL Data Warehouse stöder ett begrepp som kallas frågan etiketter. Innan du fortsätter till varje djup ska vi titta på ett exempel på en:

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

Det här sista raden taggar hello strängen 'Min fråga etiketten' toohello frågan. Detta är särskilt användbara hello etiketten är frågan kan via hello av DMV: er. Detta ger oss en mekanism tootrack ned problemet frågor och toohelp identifierar även förlopp genom ett ETL-körning.

En bra namngivningskonvention hjälper verkligen finns här. Till exempel liknande ' projekt: procedur: INSTRUKTION: kommentar ' hjälper toouniquely identifiera hello frågan i bland alla hello koden i källkontroll.

toosearch av etikett som du kan använda följande fråga som använder hello hello dynamiska hanteringsvyer:

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> Det är viktigt att du omsluter hakparenteser eller dubbla citattecken runt hello word etikett när du frågar. Etiketten är ett reserverat ord och kommer orsakade ett fel om det inte har avgränsad.
> 
> 

## <a name="next-steps"></a>Nästa steg
För fler utvecklingstips, se [utvecklingsöversikt][development overview].

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
