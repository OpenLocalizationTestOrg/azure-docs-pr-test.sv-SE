---
title: "Övervaka och förbättra prestanda - Azure SQL Database | Microsoft Docs"
description: "Azure SQL Database tillhandahåller prestandaverktyg som hjälper dig att identifiera områden som kan förbättra aktuella frågeprestanda."
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
ms.openlocfilehash: 522b932ab055978c52f085dbaa36095bb6b77962
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-and-improve-performance"></a><span data-ttu-id="e2cb3-103">Övervaka och förbättra prestanda</span><span class="sxs-lookup"><span data-stu-id="e2cb3-103">Monitor and improve performance</span></span>
<span data-ttu-id="e2cb3-104">Azure SQL-databas identifierar potentiella problem i din databas och rekommenderar åtgärder som kan förbättra prestandan för din arbetsbelastning genom att tillhandahålla intelligent prestandajustering åtgärder och rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-104">Azure SQL Database identifies potential problems in your database and recommends actions that can improve performance of your workload by providing intelligent tuning actions and recommendations.</span></span>

<span data-ttu-id="e2cb3-105">Om du vill granska databasens prestanda använder den **prestanda** panelen på översiktssidan i eller navigera till ”stöd för + felsökning” avsnittet:</span><span class="sxs-lookup"><span data-stu-id="e2cb3-105">To review your database performance, use the **Performance** tile on the Overview page, or navigate down to "Support + troubleshooting" section:</span></span>

   ![Prestanda för vyer](./media/sql-database-performance/entries.png)

<span data-ttu-id="e2cb3-107">I den ”stöd för + felsökning” avsnittet, kan du använda följande sidor:</span><span class="sxs-lookup"><span data-stu-id="e2cb3-107">In the "Support + troubleshooting" section, you can use the following pages:</span></span>


1. <span data-ttu-id="e2cb3-108">[Översikt över prestanda](#performance-overview) att övervaka prestanda för din databas.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-108">[Performance overview](#performance-overview) to monitor performance of your database.</span></span> 
2. <span data-ttu-id="e2cb3-109">[Rekommendationer](#performance-recommendations) att hitta rekommendationer som kan förbättra prestandan för din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-109">[Performance recommendations](#performance-recommendations) to find performance recommendations that can improve performance of your workload.</span></span>
3. <span data-ttu-id="e2cb3-110">[Query Performance Insight](#query-performance-insight) att hitta den övre förbrukningsfrågorna.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-110">[Query Performance Insight](#query-performance-insight) to find top resource consuming queries.</span></span>
4. <span data-ttu-id="e2cb3-111">[Automatisk justering](#automatic-tuning) så att Azure SQL Database automatiskt optimera din databas.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-111">[Automatic tuning](#automatic-tuning) to let Azure SQL Database automatically optimize your database.</span></span>

## <a name="performance-overview"></a><span data-ttu-id="e2cb3-112">Översikt över prestanda</span><span class="sxs-lookup"><span data-stu-id="e2cb3-112">Performance Overview</span></span>
<span data-ttu-id="e2cb3-113">Den här vyn innehåller en sammanfattning av databasens prestanda och hjälper dig med prestanda justera och felsökning.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-113">This view provides a summary of your database performance, and helps you with performance tuning and troubleshooting.</span></span> 

![Prestanda](./media/sql-database-performance/performance.png)

* <span data-ttu-id="e2cb3-115">Den **rekommendationer** panelen visar en uppdelning av justera rekommendationer för databasen (de tre översta rekommendationer visas om det finns flera).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-115">The **Recommendations** tile provides a breakdown of tuning recommendations for your database (top three recommendations are shown if there are more).</span></span> <span data-ttu-id="e2cb3-116">Klicka på den här panelen tar dig till  **[rekommendationer](#performance-recommendations)**.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-116">Clicking this tile takes you to **[Performance recommendations](#performance-recommendations)**.</span></span> 
* <span data-ttu-id="e2cb3-117">Den **justera aktiviteten** panelen innehåller en översikt över pågående och slutförda justera åtgärder för din databas, vilket ger en snabb överblick över historiken för inställning av aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-117">The **Tuning activity** tile provides a summary of the ongoing and completed tuning actions for your database, giving you a quick view into the history of tuning activity.</span></span> <span data-ttu-id="e2cb3-118">Klicka på den här panelen går du till fullständig prestandajustering historikvyn för din databas.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-118">Clicking this tile takes you to the full tuning history view for your database.</span></span>
* <span data-ttu-id="e2cb3-119">Den **automatisk justering** panelen visar den [automatisk justering configuration](sql-database-automatic-tuning-enable.md) för databasen (justering alternativ som automatiskt tillämpas på databasen).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-119">The **Auto-tuning** tile shows the [auto-tuning configuration](sql-database-automatic-tuning-enable.md) for your database (tuning options that are automatically applied to your database).</span></span> <span data-ttu-id="e2cb3-120">Om du klickar på den här panelen öppnas konfigurationsdialogruta automation.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-120">Clicking this tile opens the automation configuration dialog.</span></span>
* <span data-ttu-id="e2cb3-121">Den **databasfrågor** panelen visas en sammanfattning av frågeprestanda för databasen (övergripande DTU användnings- och upp resurs förbrukningsfrågorna).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-121">The **Database queries** tile shows the summary of the query performance for your database (overall DTU usage and top resource consuming queries).</span></span> <span data-ttu-id="e2cb3-122">Klicka på den här panelen tar dig till  **[Query Performance Insight](#query-performance-insight)**.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-122">Clicking this tile takes you to **[Query Performance Insight](#query-performance-insight)**.</span></span>

## <a name="performance-recommendations"></a><span data-ttu-id="e2cb3-123">Rekommendationer</span><span class="sxs-lookup"><span data-stu-id="e2cb3-123">Performance recommendations</span></span>
<span data-ttu-id="e2cb3-124">Den här sidan innehåller intelligent [justera rekommendationer](sql-database-advisor.md) som kan förbättra dina databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-124">This page provides intelligent [tuning recommendations](sql-database-advisor.md) that can improve your database's performance.</span></span> <span data-ttu-id="e2cb3-125">Följande typer av rekommendationer visas på den här sidan:</span><span class="sxs-lookup"><span data-stu-id="e2cb3-125">The following types of recommendations are shown on this page:</span></span>

* <span data-ttu-id="e2cb3-126">Rekommendationer om vilka index för att skapa eller ta bort.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-126">Recommendations on which indexes to create or drop.</span></span>
* <span data-ttu-id="e2cb3-127">Rekommendationer när schemat problem identifieras i databasen.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-127">Recommendations when schema issues are identified in the database.</span></span>
* <span data-ttu-id="e2cb3-128">Rekommendationer när frågor kan dra nytta av frågor med parametrar.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-128">Recommendations when queries can benefit from parameterized queries.</span></span>

![Prestanda](./media/sql-database-performance/recommendations.png)

<span data-ttu-id="e2cb3-130">Du kan också hitta komplett historik över justera åtgärder som har använts tidigare.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-130">You can also find complete history of tuning actions that were applied in the past.</span></span>

<span data-ttu-id="e2cb3-131">Lär dig att hitta apply rekommendationer i [söka efter och tillämpa rekommendationer](sql-database-advisor-portal.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-131">Learn how to find an apply performance recommendations in [Find and apply performance recommendations](sql-database-advisor-portal.md) article.</span></span>

## <a name="automatic-tuning"></a><span data-ttu-id="e2cb3-132">Automatisk inställning</span><span class="sxs-lookup"><span data-stu-id="e2cb3-132">Automatic tuning</span></span>
<span data-ttu-id="e2cb3-133">Azure SQL-databaser kan automatiskt finjustera databasens prestanda genom att använda [rekommendationer](sql-database-advisor.md).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-133">Azure SQL Databases can automatically tune database performance by applying [performance recommendations](sql-database-advisor.md).</span></span> <span data-ttu-id="e2cb3-134">Mer information, [automatisk justering artikel](sql-database-automatic-tuning.md).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-134">To learn more, read [Automatic tuning article](sql-database-automatic-tuning.md).</span></span> <span data-ttu-id="e2cb3-135">Om du vill aktivera den läsa [hur du aktiverar automatisk justering](sql-database-automatic-tuning-enable.md).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-135">To enable it, read [how to enable automatic tuning](sql-database-automatic-tuning-enable.md).</span></span>

## <a name="query-performance-insight"></a><span data-ttu-id="e2cb3-136">Query Performance Insight</span><span class="sxs-lookup"><span data-stu-id="e2cb3-136">Query Performance Insight</span></span>
<span data-ttu-id="e2cb3-137">[Query Performance Insight](sql-database-query-performance.md) kan du ägna mindre tid felsökning databasens prestanda genom att tillhandahålla:</span><span class="sxs-lookup"><span data-stu-id="e2cb3-137">[Query Performance Insight](sql-database-query-performance.md) allows you to spend less time troubleshooting database performance by providing:</span></span>

* <span data-ttu-id="e2cb3-138">Djupare inblick i dina databaser resursförbrukning (DTU).</span><span class="sxs-lookup"><span data-stu-id="e2cb3-138">Deeper insight into your databases resource (DTU) consumption.</span></span> 
* <span data-ttu-id="e2cb3-139">Högsta CPU förbrukningsfrågorna som potentiellt kan anpassas för bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-139">The top CPU consuming queries, which can potentially be tuned for improved performance.</span></span> 
* <span data-ttu-id="e2cb3-140">Möjlighet att öka detaljnivån till information om en fråga.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-140">The ability to drill down into the details of a query.</span></span> 

  ![instrumentpanel för prestanda](./media/sql-database-query-performance/performance.png)

<span data-ttu-id="e2cb3-142">Mer information om den här sidan finns i artikeln  **[använda Query Performance Insight](sql-database-query-performance.md)**.</span><span class="sxs-lookup"><span data-stu-id="e2cb3-142">Find more information about this page in the article **[How to use Query Performance Insight](sql-database-query-performance.md)**.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e2cb3-143">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="e2cb3-143">Additional resources</span></span>
* [<span data-ttu-id="e2cb3-144">Azure SQL Database-prestandaråd för enskilda databaser</span><span class="sxs-lookup"><span data-stu-id="e2cb3-144">Azure SQL Database performance guidance for single databases</span></span>](sql-database-performance-guidance.md)
* [<span data-ttu-id="e2cb3-145">När en elastisk pool att användas?</span><span class="sxs-lookup"><span data-stu-id="e2cb3-145">When should an elastic pool be used?</span></span>](sql-database-elastic-pool-guidance.md)

