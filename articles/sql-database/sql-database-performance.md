---
title: "aaaMonitor och förbättra prestanda - Azure SQL Database | Microsoft Docs"
description: "hello Azure SQL Database ger prestanda verktygen toohelp du identifiera områden som kan förbättra aktuella frågeprestanda."
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: monicar
ms.assetid: a60b75ac-cf27-4d73-8322-ee4d4c448aa2
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/19/2016
ms.author: sstein
ms.openlocfilehash: 84b8a1bc62698a29deb49e765f208bd7e14d0870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-improve-performance"></a><span data-ttu-id="3ed23-103">Övervaka och förbättra prestanda</span><span class="sxs-lookup"><span data-stu-id="3ed23-103">Monitor and improve performance</span></span>
<span data-ttu-id="3ed23-104">Azure SQL-databas identifierar potentiella problem i din databas och rekommenderar åtgärder som kan förbättra prestandan för din arbetsbelastning genom att tillhandahålla intelligent prestandajustering åtgärder och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="3ed23-104">Azure SQL Database identifies potential problems in your database and recommends actions that can improve performance of your workload by providing intelligent tuning actions and recommendations.</span></span>

<span data-ttu-id="3ed23-105">tooreview ditt databasprestanda, Använd hello **prestanda** panelen på översiktssidan för hello eller gå nedåt för ”stöd för + felsökning” avsnittet:</span><span class="sxs-lookup"><span data-stu-id="3ed23-105">tooreview your database performance, use hello **Performance** tile on hello Overview page, or navigate down too"Support + troubleshooting" section:</span></span>

   ![Prestanda för vyer](./media/sql-database-performance/entries.png)

<span data-ttu-id="3ed23-107">Du kan använda hello följande sidor i hello ”stöd för + felsökning” avsnittet:</span><span class="sxs-lookup"><span data-stu-id="3ed23-107">In hello "Support + troubleshooting" section, you can use hello following pages:</span></span>


1. <span data-ttu-id="3ed23-108">[Översikt över prestanda](#performance-overview) toomonitor databasens prestanda.</span><span class="sxs-lookup"><span data-stu-id="3ed23-108">[Performance overview](#performance-overview) toomonitor performance of your database.</span></span> 
2. <span data-ttu-id="3ed23-109">[Rekommendationer](#performance-recommendations) toofind rekommendationer som kan förbättra prestandan för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="3ed23-109">[Performance recommendations](#performance-recommendations) toofind performance recommendations that can improve performance of your workload.</span></span>
3. <span data-ttu-id="3ed23-110">[Query Performance Insight](#query-performance-insight) toofind översta resurs förbrukningsfrågorna.</span><span class="sxs-lookup"><span data-stu-id="3ed23-110">[Query Performance Insight](#query-performance-insight) toofind top resource consuming queries.</span></span>
4. <span data-ttu-id="3ed23-111">[Automatisk justering](#automatic-tuning) toolet Azure SQL Database automatiskt optimera din databas.</span><span class="sxs-lookup"><span data-stu-id="3ed23-111">[Automatic tuning](#automatic-tuning) toolet Azure SQL Database automatically optimize your database.</span></span>

## <a name="performance-overview"></a><span data-ttu-id="3ed23-112">Översikt över prestanda</span><span class="sxs-lookup"><span data-stu-id="3ed23-112">Performance Overview</span></span>
<span data-ttu-id="3ed23-113">Den här vyn innehåller en sammanfattning av databasens prestanda och hjälper dig med prestanda justera och felsökning.</span><span class="sxs-lookup"><span data-stu-id="3ed23-113">This view provides a summary of your database performance, and helps you with performance tuning and troubleshooting.</span></span> 

![Prestanda](./media/sql-database-performance/performance.png)

* <span data-ttu-id="3ed23-115">Hej **rekommendationer** panelen visar en uppdelning av justera rekommendationer för databasen (de tre översta rekommendationer visas om det finns flera).</span><span class="sxs-lookup"><span data-stu-id="3ed23-115">hello **Recommendations** tile provides a breakdown of tuning recommendations for your database (top three recommendations are shown if there are more).</span></span> <span data-ttu-id="3ed23-116">Klicka på den här panelen tar dig för**[rekommendationer](#performance-recommendations)**.</span><span class="sxs-lookup"><span data-stu-id="3ed23-116">Clicking this tile takes you too**[Performance recommendations](#performance-recommendations)**.</span></span> 
* <span data-ttu-id="3ed23-117">Hej **justera aktiviteten** panelen innehåller en översikt över hello pågående och slutförda justera åtgärder för din databas, vilket ger en snabb överblick över hello historiken för inställning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="3ed23-117">hello **Tuning activity** tile provides a summary of hello ongoing and completed tuning actions for your database, giving you a quick view into hello history of tuning activity.</span></span> <span data-ttu-id="3ed23-118">Klicka på den här panelen tar dig toohello fullständig justera historikvyn för din databas.</span><span class="sxs-lookup"><span data-stu-id="3ed23-118">Clicking this tile takes you toohello full tuning history view for your database.</span></span>
* <span data-ttu-id="3ed23-119">Hej **automatisk justering** panelen visar hello [automatisk justering configuration](sql-database-automatic-tuning-enable.md) för databasen (justering alternativ som är automatiskt tillämpade tooyour databas).</span><span class="sxs-lookup"><span data-stu-id="3ed23-119">hello **Auto-tuning** tile shows hello [auto-tuning configuration](sql-database-automatic-tuning-enable.md) for your database (tuning options that are automatically applied tooyour database).</span></span> <span data-ttu-id="3ed23-120">Om du klickar på den här panelen öppnas hello automation konfigurationsdialogruta.</span><span class="sxs-lookup"><span data-stu-id="3ed23-120">Clicking this tile opens hello automation configuration dialog.</span></span>
* <span data-ttu-id="3ed23-121">Hej **databasfrågor** panelen visar hello sammanfattning av hello frågeprestanda för databasen (övergripande DTU användnings- och upp resurs förbrukningsfrågorna).</span><span class="sxs-lookup"><span data-stu-id="3ed23-121">hello **Database queries** tile shows hello summary of hello query performance for your database (overall DTU usage and top resource consuming queries).</span></span> <span data-ttu-id="3ed23-122">Klicka på den här panelen tar dig för**[Query Performance Insight](#query-performance-insight)**.</span><span class="sxs-lookup"><span data-stu-id="3ed23-122">Clicking this tile takes you too**[Query Performance Insight](#query-performance-insight)**.</span></span>

## <a name="performance-recommendations"></a><span data-ttu-id="3ed23-123">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="3ed23-123">Performance recommendations</span></span>
<span data-ttu-id="3ed23-124">Den här sidan innehåller intelligent [justera rekommendationer](sql-database-advisor.md) som kan förbättra dina databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="3ed23-124">This page provides intelligent [tuning recommendations](sql-database-advisor.md) that can improve your database's performance.</span></span> <span data-ttu-id="3ed23-125">följande typer av rekommendationer hello visas på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="3ed23-125">hello following types of recommendations are shown on this page:</span></span>

* <span data-ttu-id="3ed23-126">Rekommendationer om vilka index toocreate eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="3ed23-126">Recommendations on which indexes toocreate or drop.</span></span>
* <span data-ttu-id="3ed23-127">Rekommendationer när schemat problem identifieras i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3ed23-127">Recommendations when schema issues are identified in hello database.</span></span>
* <span data-ttu-id="3ed23-128">Rekommendationer när frågor kan dra nytta av frågor med parametrar.</span><span class="sxs-lookup"><span data-stu-id="3ed23-128">Recommendations when queries can benefit from parameterized queries.</span></span>

![Prestanda](./media/sql-database-performance/recommendations.png)

<span data-ttu-id="3ed23-130">Du kan också hitta komplett historik över justera åtgärder som vidtogs i hello tidigare.</span><span class="sxs-lookup"><span data-stu-id="3ed23-130">You can also find complete history of tuning actions that were applied in hello past.</span></span>

<span data-ttu-id="3ed23-131">Lär dig hur toofind apply rekommendationer i [söka efter och tillämpa rekommendationer](sql-database-advisor-portal.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="3ed23-131">Learn how toofind an apply performance recommendations in [Find and apply performance recommendations](sql-database-advisor-portal.md) article.</span></span>

## <a name="automatic-tuning"></a><span data-ttu-id="3ed23-132">Automatisk inställning</span><span class="sxs-lookup"><span data-stu-id="3ed23-132">Automatic tuning</span></span>
<span data-ttu-id="3ed23-133">Azure SQL-databaser kan automatiskt finjustera databasens prestanda genom att använda [rekommendationer](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="3ed23-133">Azure SQL Databases can automatically tune database performance by applying [performance recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="3ed23-134">toolearn läsa fler [automatisk justering artikel](sql-database-automatic-tuning.md).</span><span class="sxs-lookup"><span data-stu-id="3ed23-134">toolearn more, read [Automatic tuning article](sql-database-automatic-tuning.md).</span></span> <span data-ttu-id="3ed23-135">tooenable, läsa [hur tooenable automatisk justering](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="3ed23-135">tooenable it, read [how tooenable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>

## <a name="query-performance-insight"></a><span data-ttu-id="3ed23-136">Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="3ed23-136">Query Performance Insight</span></span>
<span data-ttu-id="3ed23-137">[Query Performance Insight](sql-database-query-performance.md) kan du toospend mindre tid felsökning databasens prestanda genom att tillhandahålla:</span><span class="sxs-lookup"><span data-stu-id="3ed23-137">[Query Performance Insight](sql-database-query-performance.md) allows you toospend less time troubleshooting database performance by providing:</span></span>

* <span data-ttu-id="3ed23-138">Djupare inblick i dina databaser resursförbrukning (DTU).</span><span class="sxs-lookup"><span data-stu-id="3ed23-138">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="3ed23-139">hello högsta CPU förbrukningsfrågorna som potentiellt kan anpassas för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="3ed23-139">hello top CPU consuming queries, which can potentially be tuned for improved performance.</span></span> 
* <span data-ttu-id="3ed23-140">Hej möjlighet toodrill ned hello detaljer om en fråga.</span><span class="sxs-lookup"><span data-stu-id="3ed23-140">hello ability toodrill down into hello details of a query.</span></span> 

  ![instrumentpanel för prestanda](./media/sql-database-query-performance/performance.png)

<span data-ttu-id="3ed23-142">Mer information om den här sidan finns i artikeln hello  **[hur toouse Query Performance Insight](sql-database-query-performance.md)**.</span><span class="sxs-lookup"><span data-stu-id="3ed23-142">Find more information about this page in hello article **[How toouse Query Performance Insight](sql-database-query-performance.md)**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ed23-143">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3ed23-143">Additional resources</span></span>
* [<span data-ttu-id="3ed23-144">Azure SQL Database-prestandaråd för enskilda databaser</span><span class="sxs-lookup"><span data-stu-id="3ed23-144">Azure SQL Database performance guidance for single databases</span></span>](sql-database-performance-guidance.md)
* [<span data-ttu-id="3ed23-145">När en elastisk pool att användas?</span><span class="sxs-lookup"><span data-stu-id="3ed23-145">When should an elastic pool be used?</span></span>](sql-database-elastic-pool-guidance.md)

