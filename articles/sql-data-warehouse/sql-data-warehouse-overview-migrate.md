---
title: "aaaMigrate din lösning tooSQL Data Warehouse | Microsoft Docs"
description: "Riktlinjer för migrering för att skapa din lösning tooAzure SQL Data Warehouse-plattform."
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
ms.openlocfilehash: 27b51f15247603f054070f360ede7f24541c0288
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-your-solution-tooazure-sql-data-warehouse"></a><span data-ttu-id="be295-103">Migrera din lösning tooAzure SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="be295-103">Migrate your solution tooAzure SQL Data Warehouse</span></span>
<span data-ttu-id="be295-104">Se vad ingår i att migrera en befintlig databas lösning tooAzure SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="be295-104">See what's involved in migrating an existing database solution tooAzure SQL Data Warehouse.</span></span> 

## <a name="profile-your-workload"></a><span data-ttu-id="be295-105">Profilen din arbetsbelastning</span><span class="sxs-lookup"><span data-stu-id="be295-105">Profile your workload</span></span>
<span data-ttu-id="be295-106">Innan du migrerar, vill du toobe vissa SQL Data Warehouse är hello rätta lösningen för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="be295-106">Before migrating, you want toobe certain SQL Data Warehouse is hello right solution for your workload.</span></span> <span data-ttu-id="be295-107">SQL Data Warehouse är ett distribuerat system som utformats för tooperform analyser på stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="be295-107">SQL Data Warehouse is a distributed system designed tooperform analytics on large data.</span></span>  <span data-ttu-id="be295-108">Migrera tooSQL datalager kräver vissa ändringar av design som inte är för svårt toounderstand men kan ta viss tid tooimplement.</span><span class="sxs-lookup"><span data-stu-id="be295-108">Migrating tooSQL Data Warehouse requires some design changes that are not too hard toounderstand but might take some time tooimplement.</span></span> <span data-ttu-id="be295-109">Om ditt företag kräver ett informationslager i företagsklass, är hello fördelar värda hello arbete.</span><span class="sxs-lookup"><span data-stu-id="be295-109">If your business requires an enterprise-class data warehouse, hello benefits are worth hello effort.</span></span> <span data-ttu-id="be295-110">Men om du inte behöver hello kraften i SQL Data Warehouse, är det mer kostnadseffektivt toouse SQL Server eller Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="be295-110">However, if you don't need hello power of SQL Data Warehouse, it is more cost-effective toouse SQL Server or Azure SQL Database.</span></span>

<span data-ttu-id="be295-111">Överväg att använda SQL Data Warehouse när du:</span><span class="sxs-lookup"><span data-stu-id="be295-111">Consider using SQL Data Warehouse when you:</span></span>
- <span data-ttu-id="be295-112">Har en eller flera terabyte data</span><span class="sxs-lookup"><span data-stu-id="be295-112">Have one or more Terabytes of data</span></span>
- <span data-ttu-id="be295-113">Planera toorun analytics på stora mängder data</span><span class="sxs-lookup"><span data-stu-id="be295-113">Plan toorun analytics on large amounts of data</span></span>
- <span data-ttu-id="be295-114">Behöver hello möjlighet tooscale beräkning och lagring</span><span class="sxs-lookup"><span data-stu-id="be295-114">Need hello ability tooscale compute and storage</span></span> 
- <span data-ttu-id="be295-115">Vill toosave kostnader genom att pausa beräkningsresurser när de inte behövs.</span><span class="sxs-lookup"><span data-stu-id="be295-115">Want toosave costs by pausing compute resources when you don't need them.</span></span>

<span data-ttu-id="be295-116">Använd inte SQL Data Warehouse för operativa (OLTP) arbetsbelastningar som har:</span><span class="sxs-lookup"><span data-stu-id="be295-116">Don't use SQL Data Warehouse for operational (OLTP) workloads that have:</span></span>
- <span data-ttu-id="be295-117">Hög frekvens läser och skriver</span><span class="sxs-lookup"><span data-stu-id="be295-117">High frequency reads and writes</span></span>
- <span data-ttu-id="be295-118">Stort antal singleton väljer</span><span class="sxs-lookup"><span data-stu-id="be295-118">Large numbers of singleton selects</span></span>
- <span data-ttu-id="be295-119">Stora mängder enradig infogningar</span><span class="sxs-lookup"><span data-stu-id="be295-119">High volumes of single row inserts</span></span>
- <span data-ttu-id="be295-120">Rad för rad bearbetning behov</span><span class="sxs-lookup"><span data-stu-id="be295-120">Row by row processing needs</span></span>
- <span data-ttu-id="be295-121">Inkompatibla format (JSON, XML)</span><span class="sxs-lookup"><span data-stu-id="be295-121">Incompatible formats (JSON, XML)</span></span>


## <a name="plan-hello-migration"></a><span data-ttu-id="be295-122">Planera migrering av hello</span><span class="sxs-lookup"><span data-stu-id="be295-122">Plan hello migration</span></span>

<span data-ttu-id="be295-123">När du har valt toomigrate en befintlig lösning tooSQL Data Warehouse, är det viktigt tooplan hello migrering för att komma igång.</span><span class="sxs-lookup"><span data-stu-id="be295-123">Once you have decided toomigrate an existing solution tooSQL Data Warehouse, it is important tooplan hello migration before getting started.</span></span> 

<span data-ttu-id="be295-124">Ett mål för att planera tooensure dina data, din tabellscheman och koden är kompatibel med SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="be295-124">One goal of planning is tooensure your data, your table schemas, and your code are compatible with SQL Data Warehouse.</span></span> <span data-ttu-id="be295-125">Det finns vissa kompatibilitet skillnader toowork runt mellan din aktuella systemet och SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="be295-125">There are some compatibility differences toowork around between your current system and SQL Data Warehouse.</span></span> <span data-ttu-id="be295-126">Dessutom kan migrera stora mängder data tooAzure tar tid.</span><span class="sxs-lookup"><span data-stu-id="be295-126">Plus, migrating large amounts of data tooAzure takes time.</span></span> <span data-ttu-id="be295-127">Noggrann planering påskyndar hämtar dina data tooAzure.</span><span class="sxs-lookup"><span data-stu-id="be295-127">Careful planning expedites getting your data tooAzure.</span></span> 

<span data-ttu-id="be295-128">Ett annat mål av planering är toomake designen justeringar tooensure din lösning kan du utnyttja hello hög frågeprestanda SQL Data Warehouse är utformad tooprovide.</span><span class="sxs-lookup"><span data-stu-id="be295-128">Another goal of planning is toomake design adjustments tooensure your solution takes advantage of hello high query performance SQL Data Warehouse is designed tooprovide.</span></span> <span data-ttu-id="be295-129">Designa datalager för att skala introducerar olika designmönster och så traditionella metoder är inte alltid hello bäst.</span><span class="sxs-lookup"><span data-stu-id="be295-129">Designing data warehouses for scale introduces different design patterns and so traditional approaches aren't always hello best.</span></span> <span data-ttu-id="be295-130">Även om du kan göra vissa design justeringar efter migreringen, sparar gör ändringar snabbare i hello processen tid senare.</span><span class="sxs-lookup"><span data-stu-id="be295-130">Although you can make some design adjustments after migration, making changes sooner in hello process will save time later.</span></span>

<span data-ttu-id="be295-131">tooperform en framgångsrik migrering, behöver du toomigrate din tabellscheman koden och dina data.</span><span class="sxs-lookup"><span data-stu-id="be295-131">tooperform a successful migration, you need toomigrate your table schemas, your code, and your data.</span></span> <span data-ttu-id="be295-132">Information om dessa migreringsavsnitt finns:</span><span class="sxs-lookup"><span data-stu-id="be295-132">For guidance on these migration topics, see:</span></span>

-  [<span data-ttu-id="be295-133">Migrera dina scheman</span><span class="sxs-lookup"><span data-stu-id="be295-133">Migrate your schemas</span></span>](sql-data-warehouse-migrate-schema.md)
-  [<span data-ttu-id="be295-134">Migrera koden</span><span class="sxs-lookup"><span data-stu-id="be295-134">Migrate your code</span></span>](sql-data-warehouse-migrate-code.md)
-  <span data-ttu-id="be295-135">[Migrera dina data](sql-data-warehouse-migrate-data.md).</span><span class="sxs-lookup"><span data-stu-id="be295-135">[Migrate your data](sql-data-warehouse-migrate-data.md).</span></span> 

<!--
## Perform hello migration


## Deploy hello solution


## Validate hello migration

-->

## <a name="next-steps"></a><span data-ttu-id="be295-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be295-136">Next steps</span></span>
<span data-ttu-id="be295-137">hello CAT (Customer Advisory Team) har också bra vägledning SQL Data Warehouse, som de publicerar via bloggar.</span><span class="sxs-lookup"><span data-stu-id="be295-137">hello CAT (Customer Advisory Team) also has some great SQL Data Warehouse guidance, which they publish through blogs.</span></span>  <span data-ttu-id="be295-138">Ta en titt på deras artikel [migrera data tooAzure SQL Data Warehouse i praktiken] [ Migrating data tooAzure SQL Data Warehouse in practice] för ytterligare information om migrering.</span><span class="sxs-lookup"><span data-stu-id="be295-138">Take a look at their article, [Migrating data tooAzure SQL Data Warehouse in practice][Migrating data tooAzure SQL Data Warehouse in practice] for additional guidance on migration.</span></span>

<!--Image references-->

<!--Article references-->

<!--MSDN references-->

<!--Other Web references-->
[Migrating data tooAzure SQL Data Warehouse in practice]: https://blogs.msdn.microsoft.com/sqlcat/2016/08/18/migrating-data-to-azure-sql-data-warehouse-in-practice/
