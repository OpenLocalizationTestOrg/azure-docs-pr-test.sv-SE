---
title: "Migrera lösningen till SQL Data Warehouse | Microsoft Docs"
description: "Riktlinjer för migrering för att skapa din lösning för Azure SQL Data Warehouse-plattformen."
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: 198365eb-7451-4222-b99c-d1d9ef687f1b
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: migrate
ms.date: 06/27/2017
ms.author: joeyong;barbkess
ms.openlocfilehash: 771b9456e66b8a1e41f72340b695b19e2adaf793
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-your-solution-to-azure-sql-data-warehouse"></a><span data-ttu-id="a246c-103">Migrera lösningen till Azure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="a246c-103">Migrate your solution to Azure SQL Data Warehouse</span></span>
<span data-ttu-id="a246c-104">Se vad ingår i att migrera en befintlig databaslösning till Azure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a246c-104">See what's involved in migrating an existing database solution to Azure SQL Data Warehouse.</span></span> 

## <a name="profile-your-workload"></a><span data-ttu-id="a246c-105">Profilen din arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="a246c-105">Profile your workload</span></span>
<span data-ttu-id="a246c-106">Innan du migrerar, som du vill att vissa SQL Data Warehouse är den rätta lösningen för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="a246c-106">Before migrating, you want to be certain SQL Data Warehouse is the right solution for your workload.</span></span> <span data-ttu-id="a246c-107">SQL Data Warehouse är ett distribuerat system som utformats för att utföra analyser på stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="a246c-107">SQL Data Warehouse is a distributed system designed to perform analytics on large data.</span></span>  <span data-ttu-id="a246c-108">Migrera till SQL Data Warehouse kräver vissa ändringar av design som inte är hårt för att förstå men kan ta lite tid att genomföra.</span><span class="sxs-lookup"><span data-stu-id="a246c-108">Migrating to SQL Data Warehouse requires some design changes that are not too hard to understand but might take some time to implement.</span></span> <span data-ttu-id="a246c-109">Om ditt företag kräver ett informationslager i företagsklass, är fördelarna värt att arbete.</span><span class="sxs-lookup"><span data-stu-id="a246c-109">If your business requires an enterprise-class data warehouse, the benefits are worth the effort.</span></span> <span data-ttu-id="a246c-110">Om du inte behöver kraften i SQL Data Warehouse, är det dock mer kostnadseffektivt att använda SQL Server eller Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="a246c-110">However, if you don't need the power of SQL Data Warehouse, it is more cost-effective to use SQL Server or Azure SQL Database.</span></span>

<span data-ttu-id="a246c-111">Överväg att använda SQL Data Warehouse när du:</span><span class="sxs-lookup"><span data-stu-id="a246c-111">Consider using SQL Data Warehouse when you:</span></span>
- <span data-ttu-id="a246c-112">Har en eller flera terabyte data</span><span class="sxs-lookup"><span data-stu-id="a246c-112">Have one or more Terabytes of data</span></span>
- <span data-ttu-id="a246c-113">Planera att köra analytics på stora mängder data</span><span class="sxs-lookup"><span data-stu-id="a246c-113">Plan to run analytics on large amounts of data</span></span>
- <span data-ttu-id="a246c-114">Ha möjlighet att skala beräkning och lagring</span><span class="sxs-lookup"><span data-stu-id="a246c-114">Need the ability to scale compute and storage</span></span> 
- <span data-ttu-id="a246c-115">Vill du spara kostnader genom att pausa beräkningsresurser när de inte behövs.</span><span class="sxs-lookup"><span data-stu-id="a246c-115">Want to save costs by pausing compute resources when you don't need them.</span></span>

<span data-ttu-id="a246c-116">Använd inte SQL Data Warehouse för operativa (OLTP) arbetsbelastningar som har:</span><span class="sxs-lookup"><span data-stu-id="a246c-116">Don't use SQL Data Warehouse for operational (OLTP) workloads that have:</span></span>
- <span data-ttu-id="a246c-117">Hög frekvens läser och skriver</span><span class="sxs-lookup"><span data-stu-id="a246c-117">High frequency reads and writes</span></span>
- <span data-ttu-id="a246c-118">Stort antal singleton väljer</span><span class="sxs-lookup"><span data-stu-id="a246c-118">Large numbers of singleton selects</span></span>
- <span data-ttu-id="a246c-119">Stora mängder enradig infogningar</span><span class="sxs-lookup"><span data-stu-id="a246c-119">High volumes of single row inserts</span></span>
- <span data-ttu-id="a246c-120">Rad för rad bearbetning behov</span><span class="sxs-lookup"><span data-stu-id="a246c-120">Row by row processing needs</span></span>
- <span data-ttu-id="a246c-121">Inkompatibla format (JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="a246c-121">Incompatible formats (JSON, XML)</span></span>


## <a name="plan-the-migration"></a><span data-ttu-id="a246c-122">Planera för migrering</span><span class="sxs-lookup"><span data-stu-id="a246c-122">Plan the migration</span></span>

<span data-ttu-id="a246c-123">När du har valt att migrera en befintlig lösning till SQL Data Warehouse, är det viktigt att planera för migrering för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="a246c-123">Once you have decided to migrate an existing solution to SQL Data Warehouse, it is important to plan the migration before getting started.</span></span> 

<span data-ttu-id="a246c-124">Ett mål av planering är att säkerställa att dina data, din tabellscheman och koden är kompatibla med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a246c-124">One goal of planning is to ensure your data, your table schemas, and your code are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="a246c-125">Det finns vissa skillnader kompatibilitet undvika mellan din aktuella systemet och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="a246c-125">There are some compatibility differences to work around between your current system and SQL Data Warehouse.</span></span> <span data-ttu-id="a246c-126">Dessutom att migrera stora mängder data till Azure tar tid.</span><span class="sxs-lookup"><span data-stu-id="a246c-126">Plus, migrating large amounts of data to Azure takes time.</span></span> <span data-ttu-id="a246c-127">Noggrann planering påskyndar hämtar dina data till Azure.</span><span class="sxs-lookup"><span data-stu-id="a246c-127">Careful planning expedites getting your data to Azure.</span></span> 

<span data-ttu-id="a246c-128">Ett annat mål av planering är att justera design så din lösning drar nytta av de hög frågeprestanda SQL Data Warehouse är utformad att ge.</span><span class="sxs-lookup"><span data-stu-id="a246c-128">Another goal of planning is to make design adjustments to ensure your solution takes advantage of the high query performance SQL Data Warehouse is designed to provide.</span></span> <span data-ttu-id="a246c-129">Designa datalager för att skala introducerar olika design mönster och så traditionella metoder är inte alltid bäst.</span><span class="sxs-lookup"><span data-stu-id="a246c-129">Designing data warehouses for scale introduces different design patterns and so traditional approaches aren't always the best.</span></span> <span data-ttu-id="a246c-130">Även om du kan göra vissa design justeringar efter migreringen, sparar att göra ändringar i processen snabbare tid senare.</span><span class="sxs-lookup"><span data-stu-id="a246c-130">Although you can make some design adjustments after migration, making changes sooner in the process will save time later.</span></span>

<span data-ttu-id="a246c-131">För att genomföra en framgångsrik migrering, måste du migrera din tabellscheman, koden och dina data.</span><span class="sxs-lookup"><span data-stu-id="a246c-131">To perform a successful migration, you need to migrate your table schemas, your code, and your data.</span></span> <span data-ttu-id="a246c-132">Information om dessa migreringsavsnitt finns:</span><span class="sxs-lookup"><span data-stu-id="a246c-132">For guidance on these migration topics, see:</span></span>

-  [<span data-ttu-id="a246c-133">Migrera dina scheman</span><span class="sxs-lookup"><span data-stu-id="a246c-133">Migrate your schemas</span></span>](sql-data-warehouse-migrate-schema.md)
-  [<span data-ttu-id="a246c-134">Migrera koden</span><span class="sxs-lookup"><span data-stu-id="a246c-134">Migrate your code</span></span>](sql-data-warehouse-migrate-code.md)
-  <span data-ttu-id="a246c-135">[Migrera dina data](sql-data-warehouse-migrate-data.md).</span><span class="sxs-lookup"><span data-stu-id="a246c-135">[Migrate your data](sql-data-warehouse-migrate-data.md).</span></span> 

<!--
## Perform the migration


## Deploy the solution


## Validate the migration

-->

## <a name="next-steps"></a><span data-ttu-id="a246c-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a246c-136">Next steps</span></span>
<span data-ttu-id="a246c-137">CAT (Customer Advisory Team) har också bra vägledning SQL Data Warehouse, som de publicerar via bloggar.</span><span class="sxs-lookup"><span data-stu-id="a246c-137">The CAT (Customer Advisory Team) also has some great SQL Data Warehouse guidance, which they publish through blogs.</span></span>  <span data-ttu-id="a246c-138">Ta en titt på deras artikel [migrera data till Azure SQL Data Warehouse i praktiken] [ Migrating data to Azure SQL Data Warehouse in practice] för ytterligare information om migrering.</span><span class="sxs-lookup"><span data-stu-id="a246c-138">Take a look at their article, [Migrating data to Azure SQL Data Warehouse in practice][Migrating data to Azure SQL Data Warehouse in practice] for additional guidance on migration.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data to Azure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
