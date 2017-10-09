---
title: aaaMonitor XTP InMemory-lagringen | Microsoft Docs
description: "Uppskattning och övervaka XTP InMemory-lagringen använder kapacitet. Åtgärda felet kapacitet 41823"
services: sql-database
documentationcenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: b617308e-692c-4938-8fa2-070034a3ecef
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: jodebrui
ms.openlocfilehash: fcb17bd8e9ebef4862d4b55bf5a79b45b9419fca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-in-memory-oltp-storage"></a><span data-ttu-id="99882-103">Övervaka minnesintern OLTP-lagring</span><span class="sxs-lookup"><span data-stu-id="99882-103">Monitor In-Memory OLTP Storage</span></span>
<span data-ttu-id="99882-104">När du använder [Minnesintern OLTP](sql-database-in-memory.md), minnesoptimerade tabeller och tabellvariabler finns i InMemory-OLTP-lagring.</span><span class="sxs-lookup"><span data-stu-id="99882-104">When using [In-Memory OLTP](sql-database-in-memory.md), data in memory-optimized tables and table variables resides in In-Memory OLTP storage.</span></span> <span data-ttu-id="99882-105">Varje premiumnivån har en maximal Minnesintern OLTP lagringsstorlek, som dokumenteras i hello [SQL Database servicenivåer artikel](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span><span class="sxs-lookup"><span data-stu-id="99882-105">Each Premium service tier has a maximum In-Memory OLTP storage size, which is documented in hello [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels).</span></span> <span data-ttu-id="99882-106">När den här gränsen överskrids, infoga och uppdatera operations startar (med fel 41823).</span><span class="sxs-lookup"><span data-stu-id="99882-106">Once this limit is exceeded, insert and update operations may start failing (with error 41823).</span></span> <span data-ttu-id="99882-107">Då kommer du behöver tooeither ta bort data tooreclaim minne eller uppgradera hello prestandanivån för din databas.</span><span class="sxs-lookup"><span data-stu-id="99882-107">At that point you will need tooeither delete data tooreclaim memory, or upgrade hello performance tier of your database.</span></span>

## <a name="determine-whether-data-will-fit-within-hello-in-memory-storage-cap"></a><span data-ttu-id="99882-108">Avgöra om data får plats inom hello InMemory-lagringen linjeslut</span><span class="sxs-lookup"><span data-stu-id="99882-108">Determine whether data will fit within hello in-memory storage cap</span></span>
<span data-ttu-id="99882-109">Fastställa hello lagring cap: Kontakta hello [SQL Database servicenivåer artikel](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) för hello lagring caps av hello olika Premium tjänstnivåer.</span><span class="sxs-lookup"><span data-stu-id="99882-109">Determine hello storage cap: consult hello [SQL Database Service Tiers article](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) for hello storage caps of hello different Premium service tiers.</span></span>

<span data-ttu-id="99882-110">Uppskatta minne krav för en minnesoptimerad tabell fungerar hello samma har sätt för SQL Server som den i Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="99882-110">Estimating memory requirements for a memory-optimized table works hello same way for SQL Server as it does in Azure SQL Database.</span></span> <span data-ttu-id="99882-111">Ta ett par minuter tooreview avsnittet på [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span><span class="sxs-lookup"><span data-stu-id="99882-111">Take a few minutes tooreview that topic on [MSDN](https://msdn.microsoft.com/library/dn282389.aspx).</span></span>

<span data-ttu-id="99882-112">Observera att hello tabell och variabeln tabellraderna samt index, räknas in Hej max användaren datastorlek.</span><span class="sxs-lookup"><span data-stu-id="99882-112">Note that hello table and table variable rows, as well as indexes, count toward hello max user data size.</span></span> <span data-ttu-id="99882-113">ALTER TABLE måste dessutom tillräckligt med utrymme toocreate en ny version av hello hela tabellen och dess index.</span><span class="sxs-lookup"><span data-stu-id="99882-113">In addition, ALTER TABLE needs enough room toocreate a new version of hello entire table and its indexes.</span></span>

## <a name="monitoring-and-alerting"></a><span data-ttu-id="99882-114">Övervakning och avisering</span><span class="sxs-lookup"><span data-stu-id="99882-114">Monitoring and alerting</span></span>
<span data-ttu-id="99882-115">Du kan övervaka InMemory-lagringsanvändning som en procentandel av hello [lagring fjärrskrivbordsanslutning för din prestandanivån](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) i hello Azure [portal](https://portal.azure.com/):</span><span class="sxs-lookup"><span data-stu-id="99882-115">You can monitor in-memory storage use as a percentage of hello [storage cap for your performance tier](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) in hello Azure [portal](https://portal.azure.com/):</span></span> 

* <span data-ttu-id="99882-116">Leta upp hello resurs användning rutan på hello databasbladet och klicka på Redigera.</span><span class="sxs-lookup"><span data-stu-id="99882-116">On hello Database blade, locate hello Resource utilization box and click on Edit.</span></span>
* <span data-ttu-id="99882-117">Välj hello mått `In-Memory OLTP Storage percentage`.</span><span class="sxs-lookup"><span data-stu-id="99882-117">Then select hello metric `In-Memory OLTP Storage percentage`.</span></span>
* <span data-ttu-id="99882-118">tooadd en avisering klickar du på hello resursutnyttjande rutan tooopen hello mått bladet och sedan klicka på Lägg till avisering.</span><span class="sxs-lookup"><span data-stu-id="99882-118">tooadd an alert, click on hello Resource Utilization box tooopen hello Metric blade, then click on Add alert.</span></span>

<span data-ttu-id="99882-119">Eller Använd hello följande fråga tooshow hello InMemory-lagringsanvändningen:</span><span class="sxs-lookup"><span data-stu-id="99882-119">Or use hello following query tooshow hello in-memory storage utilization:</span></span>

    SELECT xtp_storage_percent FROM sys.dm_db_resource_stats


## <a name="correct-out-of-memory-situations---error-41823"></a><span data-ttu-id="99882-120">Korrigera minnet är slut situationer - fel 41823</span><span class="sxs-lookup"><span data-stu-id="99882-120">Correct out-of-memory situations - Error 41823</span></span>
<span data-ttu-id="99882-121">Kör minnet är slut resultat i INSERT-, UPDATE- och skapa åtgärder misslyckas med felmeddelandet 41823.</span><span class="sxs-lookup"><span data-stu-id="99882-121">Running out-of-memory results in INSERT, UPDATE, and CREATE operations failing with error message 41823.</span></span>

<span data-ttu-id="99882-122">Felmeddelandet 41823 innebär att hello minnesoptimerade tabeller och tabellvariabler har överskridit hello maxstorleken.</span><span class="sxs-lookup"><span data-stu-id="99882-122">Error message 41823 indicates that hello memory-optimized tables and table variables have exceeded hello maximum size.</span></span>

<span data-ttu-id="99882-123">tooresolve felet, antingen:</span><span class="sxs-lookup"><span data-stu-id="99882-123">tooresolve this error, either:</span></span>

* <span data-ttu-id="99882-124">Ta bort data från hello minnesoptimerade tabeller, potentiellt avlasta hello data tootraditional, diskbaserade tabeller. eller,</span><span class="sxs-lookup"><span data-stu-id="99882-124">Delete data from hello memory-optimized tables, potentially offloading hello data tootraditional, disk-based tables; or,</span></span>
* <span data-ttu-id="99882-125">Uppgradera hello service tier tooone med tillräckligt med InMemory-lagringen för hello data behöver du tookeep i minnesoptimerade tabeller.</span><span class="sxs-lookup"><span data-stu-id="99882-125">Upgrade hello service tier tooone with enough in-memory storage for hello data you need tookeep in memory-optimized tables.</span></span>

## <a name="next-steps"></a><span data-ttu-id="99882-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99882-126">Next steps</span></span>
<span data-ttu-id="99882-127">För att övervaka information, se [övervakning Azure SQL Database med dynamiska hanteringsvyer](sql-database-monitoring-with-dmvs.md).</span><span class="sxs-lookup"><span data-stu-id="99882-127">For monitoring guidance, see [Monitoring Azure SQL Database using dynamic management views](sql-database-monitoring-with-dmvs.md).</span></span>
