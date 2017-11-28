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
# <a name="use-labels-tooinstrument-queries-in-sql-data-warehouse"></a><span data-ttu-id="85846-103">Använda etiketter tooinstrument frågor i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="85846-103">Use labels tooinstrument queries in SQL Data Warehouse</span></span>
<span data-ttu-id="85846-104">SQL Data Warehouse stöder ett begrepp som kallas frågan etiketter.</span><span class="sxs-lookup"><span data-stu-id="85846-104">SQL Data Warehouse supports a concept called query labels.</span></span> <span data-ttu-id="85846-105">Innan du fortsätter till varje djup ska vi titta på ett exempel på en:</span><span class="sxs-lookup"><span data-stu-id="85846-105">Before going into any depth let's look at an example of one:</span></span>

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

<span data-ttu-id="85846-106">Det här sista raden taggar hello strängen 'Min fråga etiketten' toohello frågan.</span><span class="sxs-lookup"><span data-stu-id="85846-106">This last line tags hello string 'My Query Label' toohello query.</span></span> <span data-ttu-id="85846-107">Detta är särskilt användbara hello etiketten är frågan kan via hello av DMV: er.</span><span class="sxs-lookup"><span data-stu-id="85846-107">This is particularly helpful as hello label is query-able through hello DMVs.</span></span> <span data-ttu-id="85846-108">Detta ger oss en mekanism tootrack ned problemet frågor och toohelp identifierar även förlopp genom ett ETL-körning.</span><span class="sxs-lookup"><span data-stu-id="85846-108">This provides us with a mechanism tootrack down problem queries and also toohelp identify progress through an ETL run.</span></span>

<span data-ttu-id="85846-109">En bra namngivningskonvention hjälper verkligen finns här.</span><span class="sxs-lookup"><span data-stu-id="85846-109">A good naming convention really helps here.</span></span> <span data-ttu-id="85846-110">Till exempel liknande ' projekt: procedur: INSTRUKTION: kommentar ' hjälper toouniquely identifiera hello frågan i bland alla hello koden i källkontroll.</span><span class="sxs-lookup"><span data-stu-id="85846-110">For example something like ' PROJECT : PROCEDURE : STATEMENT : COMMENT' would help toouniquely identify hello query in amongst all hello code in source control.</span></span>

<span data-ttu-id="85846-111">toosearch av etikett som du kan använda följande fråga som använder hello hello dynamiska hanteringsvyer:</span><span class="sxs-lookup"><span data-stu-id="85846-111">toosearch by label you can use hello following query that uses hello dynamic management views:</span></span>

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> <span data-ttu-id="85846-112">Det är viktigt att du omsluter hakparenteser eller dubbla citattecken runt hello word etikett när du frågar.</span><span class="sxs-lookup"><span data-stu-id="85846-112">It is essential that you wrap square brackets or double quotes around hello word label when querying.</span></span> <span data-ttu-id="85846-113">Etiketten är ett reserverat ord och kommer orsakade ett fel om det inte har avgränsad.</span><span class="sxs-lookup"><span data-stu-id="85846-113">Label is a reserved word and will caused an error if it has not been delimited.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="85846-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85846-114">Next steps</span></span>
<span data-ttu-id="85846-115">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="85846-115">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
