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
# <a name="dynamic-sql-in-sql-data-warehouse"></a><span data-ttu-id="16d39-103">Dynamisk SQL i SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="16d39-103">Dynamic SQL in SQL Data Warehouse</span></span>
<span data-ttu-id="16d39-104">När du utvecklar programkod för SQL Data Warehouse som du kanske måste toouse dynamisk sql toohelp levererar flexibla, allmän och modulär lösningar.</span><span class="sxs-lookup"><span data-stu-id="16d39-104">When developing application code for SQL Data Warehouse you may need toouse dynamic sql toohelp deliver flexible, generic and modular solutions.</span></span> <span data-ttu-id="16d39-105">SQL Data Warehouse stöder inte blob-datatyper just nu.</span><span class="sxs-lookup"><span data-stu-id="16d39-105">SQL Data Warehouse does not support blob data types at this time.</span></span> <span data-ttu-id="16d39-106">Detta kan begränsa hello storleken på din strängar som blob-typer innehålla både varchar(max) och nvarchar(max) typer.</span><span class="sxs-lookup"><span data-stu-id="16d39-106">This may limit hello size of your strings as blob types include both varchar(max) and nvarchar(max) types.</span></span> <span data-ttu-id="16d39-107">Om du har använt dessa typer i din programkod när du skapar mycket stora strängar, behöver du toobreak hello koden i segment och Använd hello EXEC-instruktion i stället.</span><span class="sxs-lookup"><span data-stu-id="16d39-107">If you have used these types in your application code when building very large strings, you will need toobreak hello code into chunks and use hello EXEC statement instead.</span></span>

<span data-ttu-id="16d39-108">Ett enkelt exempel:</span><span class="sxs-lookup"><span data-stu-id="16d39-108">A simple example:</span></span>

```sql
DECLARE @sql_fragment1 VARCHAR(8000)=' SELECT name '
,       @sql_fragment2 VARCHAR(8000)=' FROM sys.system_views '
,       @sql_fragment3 VARCHAR(8000)=' WHERE name like ''%table%''';

EXEC( @sql_fragment1 + @sql_fragment2 + @sql_fragment3);
```

<span data-ttu-id="16d39-109">Om hello sträng är kort kan du använda [sp_executesql] [ sp_executesql] som vanligt.</span><span class="sxs-lookup"><span data-stu-id="16d39-109">If hello string is short you can use [sp_executesql][sp_executesql] as normal.</span></span>

> [!NOTE]
> <span data-ttu-id="16d39-110">Instruktioner som körs som dynamiska SQL kommer fortfarande att ämne tooall TSQL valideringsregler.</span><span class="sxs-lookup"><span data-stu-id="16d39-110">Statements executed as dynamic SQL will still be subject tooall TSQL validation rules.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="16d39-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16d39-111">Next steps</span></span>
<span data-ttu-id="16d39-112">För fler utvecklingstips, se [utvecklingsöversikt][development overview].</span><span class="sxs-lookup"><span data-stu-id="16d39-112">For more development tips, see [development overview][development overview].</span></span>

<!--Image references-->

<!--Article references-->
[development overview]: sql-data-warehouse-overview-develop.md

<!--MSDN references-->
[sp_executesql]: https://msdn.microsoft.com/library/ms188001.aspx

<!--Other Web references-->
