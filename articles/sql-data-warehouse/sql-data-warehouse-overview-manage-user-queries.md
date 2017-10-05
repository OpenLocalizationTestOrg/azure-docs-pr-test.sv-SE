---
title: "Övervaka användarfrågor i Azure SQL Data Warehouse | Microsoft Docs"
description: "Översikt över de överväganden, bästa praxis och uppgifter för övervakning av användarfrågor i Azure SQL Data Warehouse"
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 1d0960db-5dcf-4a08-b1dc-6c5d3d5a616d
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: joeyong;barbkess
ms.openlocfilehash: 65509a65c2b34553822cc02d7a7fa5a614adc57f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-user-queries-in-azure-sql-data-warehouse"></a><span data-ttu-id="580a9-103">Övervaka användarfrågor i Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="580a9-103">Monitor user queries in Azure SQL Data Warehouse</span></span>
<span data-ttu-id="580a9-104">Översikt över de överväganden, bästa praxis och uppgifter för övervakning av användarfrågor i SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="580a9-104">Overview of the considerations, best practices, and tasks for monitoring user queries in SQL Data Warehouse.</span></span>

| <span data-ttu-id="580a9-105">Kategori</span><span class="sxs-lookup"><span data-stu-id="580a9-105">Category</span></span> | <span data-ttu-id="580a9-106">Uppgiften eller ersättning</span><span class="sxs-lookup"><span data-stu-id="580a9-106">Task or consideration</span></span> | <span data-ttu-id="580a9-107">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="580a9-107">Description</span></span> |
|:--- |:--- |:--- |
| <span data-ttu-id="580a9-108">Långsam prestanda</span><span class="sxs-lookup"><span data-stu-id="580a9-108">Slow performance</span></span> |<span data-ttu-id="580a9-109">Hitta en tidskrävande användarfrågan</span><span class="sxs-lookup"><span data-stu-id="580a9-109">Find a long-running user query</span></span> |<span data-ttu-id="580a9-110">[Hitta tidskrävande frågor][Find long-running queries]</span><span class="sxs-lookup"><span data-stu-id="580a9-110">[Find long-running queries][Find long-running queries]</span></span> |
| <span data-ttu-id="580a9-111">Samtidighet</span><span class="sxs-lookup"><span data-stu-id="580a9-111">Concurrency</span></span> |<span data-ttu-id="580a9-112">Tilldela användarfrågor samtidiga resurser</span><span class="sxs-lookup"><span data-stu-id="580a9-112">Assign concurrent resources to user queries</span></span> |<span data-ttu-id="580a9-113">[Hantering av samtidighet och arbetsbelastning][Concurrency and workload management]</span><span class="sxs-lookup"><span data-stu-id="580a9-113">[Concurrency and workload management][Concurrency and workload management]</span></span> |

## <a name="next-steps"></a><span data-ttu-id="580a9-114">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="580a9-114">Next steps</span></span>
<span data-ttu-id="580a9-115">Fler management tips, gå till den [översikt över][Management overview].</span><span class="sxs-lookup"><span data-stu-id="580a9-115">For more management tips, head over to the [Management overview][Management overview].</span></span>

<!--Image references-->

<!--Article references-->
[Find long-running queries]: sql-data-warehouse-manage-monitor.md
[Concurrency and workload management]: sql-data-warehouse-develop-concurrency.md
[Management overview]: sql-data-warehouse-overview-manage.md

<!--MSDN references-->


<!--Other Web references-->
