---
title: "Använda etiketter på betalningsinstrument frågor i SQL Data Warehouse | Microsoft Docs"
description: "Tips för att använda etiketter på betalningsinstrument frågor i Azure SQL Data Warehouse för utveckling av lösningar."
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
ms.openlocfilehash: 9e75bbe528a427724a623305fbd45e2277e9d0af
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-labels-to-instrument-queries-in-sql-data-warehouse"></a><span data-ttu-id="cdc8f-103">Använd etiketter till betalningsinstrument frågor i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="cdc8f-103">Use labels to instrument queries in SQL Data Warehouse</span></span>
<span data-ttu-id="cdc8f-104">SQL Data Warehouse stöder ett begrepp som kallas frågan etiketter.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-104">SQL Data Warehouse supports a concept called query labels.</span></span> <span data-ttu-id="cdc8f-105">Innan du fortsätter till varje djup ska vi titta på ett exempel på en:</span><span class="sxs-lookup"><span data-stu-id="cdc8f-105">Before going into any depth let's look at an example of one:</span></span>

```sql
SELECT *
FROM sys.tables
OPTION (LABEL = 'My Query Label')
;
```

<span data-ttu-id="cdc8f-106">Det här sista raden taggar strängen 'Min fråga etiketten' i frågan.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-106">This last line tags the string 'My Query Label' to the query.</span></span> <span data-ttu-id="cdc8f-107">Detta är särskilt användbara etiketten är frågan kan via de av DMV: er.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-107">This is particularly helpful as the label is query-able through the DMVs.</span></span> <span data-ttu-id="cdc8f-108">Detta ger oss en mekanism för att spåra problem frågor och för att identifiera förlopp genom ett ETL-körning.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-108">This provides us with a mechanism to track down problem queries and also to help identify progress through an ETL run.</span></span>

<span data-ttu-id="cdc8f-109">En bra namngivningskonvention hjälper verkligen finns här.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-109">A good naming convention really helps here.</span></span> <span data-ttu-id="cdc8f-110">Till exempel liknande ' projekt: procedur: INSTRUKTIONEN: kommentar ' skulle bidra till att identifiera frågan i bland all kod i källkontroll.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-110">For example something like ' PROJECT : PROCEDURE : STATEMENT : COMMENT' would help to uniquely identify the query in amongst all the code in source control.</span></span>

<span data-ttu-id="cdc8f-111">Du kan använda följande fråga som använder de dynamiska hanteringsvyer om du vill söka efter etikett:</span><span class="sxs-lookup"><span data-stu-id="cdc8f-111">To search by label you can use the following query that uses the dynamic management views:</span></span>

```sql
SELECT  *
FROM    sys.dm_pdw_exec_requests r
WHERE   r.[label] = 'My Query Label'
;
```

> [!NOTE]
> <span data-ttu-id="cdc8f-112">Det är viktigt att du omsluter hakparenteser eller dubbla citattecken runt word etiketten när du frågar.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-112">It is essential that you wrap square brackets or double quotes around the word label when querying.</span></span> <span data-ttu-id="cdc8f-113">Etiketten är ett reserverat ord och kommer orsakade ett fel om det inte har avgränsad.</span><span class="sxs-lookup"><span data-stu-id="cdc8f-113">Label is a reserved word and will caused an error if it has not been delimited.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="cdc8f-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cdc8f-114">Next steps</span></span>
<span data-ttu-id="cdc8f-115">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="cdc8f-115">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->

<!--Other Web references-->
