---
title: "Felsöka problem med prestanda och optimera din databas | Microsoft Docs"
description: "Tillämpa rekommendationer till din SQL-databas, samt Rensa så få insikter om prestanda för frågor som körs mot databasen"
metakeywords: azure sql database performance monitoring recommendation
services: sql-database
documentationcenter: 
manager: jhubbard
author: jan-eng
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,monitor & tune
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: janeng
ms.openlocfilehash: f9ae96cdc80c347593f229cb2fce3f2d4d8e7caf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a><span data-ttu-id="d1b09-103">Felsöka problem med prestanda och optimera din databas</span><span class="sxs-lookup"><span data-stu-id="d1b09-103">Troubleshoot performance issues and optimize your database</span></span>

<span data-ttu-id="d1b09-104">Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="d1b09-104">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="d1b09-105">I den här kursen lär du dig:</span><span class="sxs-lookup"><span data-stu-id="d1b09-105">In this tutorial you learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d1b09-106">Granska, tillämpa och återställa improvement rekommendationer</span><span class="sxs-lookup"><span data-stu-id="d1b09-106">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="d1b09-107">Hitta frågor med hög resursutnyttjande</span><span class="sxs-lookup"><span data-stu-id="d1b09-107">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="d1b09-108">Hitta tidskrävande frågor</span><span class="sxs-lookup"><span data-stu-id="d1b09-108">Find long running queries</span></span>

> <span data-ttu-id="d1b09-109">Du behöver en kontinuerlig arbetsbelastning på en databas med prestandaproblem – saknas ett index till exempel för att få en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="d1b09-109">You need a continuous workload on a database with performance issues – missing an index for example to receive a recommendation.</span></span>
>

## <a name="log-in-to-the-azure-portal"></a><span data-ttu-id="d1b09-110">Logga in på Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d1b09-110">Log in to the Azure portal</span></span>

<span data-ttu-id="d1b09-111">Logga in på [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d1b09-111">Log in to the [Azure portal](https://portal.azure.com/).</span></span>

## <a name="review-and-apply-a-recommendation"></a><span data-ttu-id="d1b09-112">Granska och Använd en rekommendation</span><span class="sxs-lookup"><span data-stu-id="d1b09-112">Review and apply a recommendation</span></span>

<span data-ttu-id="d1b09-113">Följ dessa steg om du vill använda en rekommendation från systemet för databasen:</span><span class="sxs-lookup"><span data-stu-id="d1b09-113">Follow these steps to apply a recommendation from the system for your database:</span></span>

1. <span data-ttu-id="d1b09-114">Klicka på den **rekommendationer** -menyn i databasbladet.</span><span class="sxs-lookup"><span data-stu-id="d1b09-114">Click the **Performance recommendations** menu in the database blade.</span></span>

    ![rekommendation för prestanda](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. <span data-ttu-id="d1b09-116">Välj en aktiv rekommendation från listan över rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="d1b09-116">From the list of recommendations, select an active recommendation.</span></span> <span data-ttu-id="d1b09-117">I det här exemplet skapar Index.</span><span class="sxs-lookup"><span data-stu-id="d1b09-117">In this example, Create Index.</span></span>

    ![Välj rekommendation](./media/sql-database-performance-tutorial/create_index.png)

3. <span data-ttu-id="d1b09-119">Använda rekommendationen genom att klicka på den **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1b09-119">Apply the recommendation by clicking the **Apply** button.</span></span> <span data-ttu-id="d1b09-120">Du kan också granska information om en rekommendation och se T-SQL-skript för att köras genom att klicka på **Visa skript** knappen.</span><span class="sxs-lookup"><span data-stu-id="d1b09-120">Optionally, review the recommendation details and see the T-SQL script to  be executed by clicking on **View Script** button.</span></span>

    ![Tillämpa rekommendation](./media/sql-database-performance-tutorial/apply.png)

4. <span data-ttu-id="d1b09-122">[Valfritt] Aktivera automatisk justering för rekommendationer tillämpas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d1b09-122">[Optional] Enable automatic tuning for recommendations to be applied automatically.</span></span>

    ![automatisk justering](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a><span data-ttu-id="d1b09-124">Återställa en rekommendation</span><span class="sxs-lookup"><span data-stu-id="d1b09-124">Revert a recommendation</span></span>

<span data-ttu-id="d1b09-125">Database Advisor övervakar varje rekommendation implementeras.</span><span class="sxs-lookup"><span data-stu-id="d1b09-125">The Database Advisor monitors every recommendation implemented.</span></span> <span data-ttu-id="d1b09-126">Om en rekommendation inte förbättrar arbetsbelastningen som den kommer att återställas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="d1b09-126">If a recommendation doesn't improve the workload it will be automatically reverted.</span></span> <span data-ttu-id="d1b09-127">Manuellt återställa en rekommendation är möjligt, men inte behövs i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="d1b09-127">Manually reverting a recommendation is possible, but not necessary in most cases.</span></span> <span data-ttu-id="d1b09-128">Återställa en rekommendation:</span><span class="sxs-lookup"><span data-stu-id="d1b09-128">To revert a recommendation:</span></span>

1. <span data-ttu-id="d1b09-129">Gå till menyn prestanda rekommendationer och välj något av rekommendationerna som används.</span><span class="sxs-lookup"><span data-stu-id="d1b09-129">Go to the performance recommendations menu and select one of the applied recommendations.</span></span>

    ![Välj rekommendation](./media/sql-database-performance-tutorial/select.png)

2. <span data-ttu-id="d1b09-131">Klicka i Detaljvyn **Återställ**.</span><span class="sxs-lookup"><span data-stu-id="d1b09-131">In the details view, click **Revert**.</span></span>

    ![återställa rekommendation](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-the-query-that-consumes-the-most-resources"></a><span data-ttu-id="d1b09-133">Leta upp frågan som förbrukar mest resurser</span><span class="sxs-lookup"><span data-stu-id="d1b09-133">Find the query that consumes the most resources</span></span>

<span data-ttu-id="d1b09-134">Följ dessa steg om du vill leta upp frågan som förbrukar mest resurser:</span><span class="sxs-lookup"><span data-stu-id="d1b09-134">Follow these steps to find the query consuming the most resources:</span></span>

1. <span data-ttu-id="d1b09-135">Klicka på den **Query Performance Insight** -menyn i databasbladet.</span><span class="sxs-lookup"><span data-stu-id="d1b09-135">Click on the **Query Performance Insight** menu in the database blade.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. <span data-ttu-id="d1b09-137">Välj en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="d1b09-137">Select a resource type.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/select_resource_type.png)

3. <span data-ttu-id="d1b09-139">Välj den första frågan i tabellen.</span><span class="sxs-lookup"><span data-stu-id="d1b09-139">Select the first query in the table.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/select_query.png)

4. <span data-ttu-id="d1b09-141">Granska informationen för frågan.</span><span class="sxs-lookup"><span data-stu-id="d1b09-141">Review the query details.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-the-longest-running-query"></a><span data-ttu-id="d1b09-143">Leta upp längst körningstid frågan</span><span class="sxs-lookup"><span data-stu-id="d1b09-143">Find the longest running query</span></span>

1. <span data-ttu-id="d1b09-144">Gå till Query Performance Insight och välj den **tidskrävande frågor** fliken.</span><span class="sxs-lookup"><span data-stu-id="d1b09-144">Go to Query Performance Insight and select the **Long running queries** tab.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/long_running.png)

3. <span data-ttu-id="d1b09-146">Välj den första frågan i tabellen.</span><span class="sxs-lookup"><span data-stu-id="d1b09-146">Select the first query in the table.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/select_first_query.png)

4. <span data-ttu-id="d1b09-148">Granska informationen för frågan.</span><span class="sxs-lookup"><span data-stu-id="d1b09-148">Review the query details.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a><span data-ttu-id="d1b09-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1b09-150">Next steps</span></span> 
<span data-ttu-id="d1b09-151">Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="d1b09-151">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="d1b09-152">I den här kursen har du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="d1b09-152">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="d1b09-153">Granska, tillämpa och återställa improvement rekommendationer</span><span class="sxs-lookup"><span data-stu-id="d1b09-153">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="d1b09-154">Hitta frågor med hög resursutnyttjande</span><span class="sxs-lookup"><span data-stu-id="d1b09-154">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="d1b09-155">Hitta tidskrävande frågor</span><span class="sxs-lookup"><span data-stu-id="d1b09-155">Find long running queries</span></span>

[<span data-ttu-id="d1b09-156">SQL Database prestanda över tips</span><span class="sxs-lookup"><span data-stu-id="d1b09-156">SQL Database performance tuning tips</span></span>](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
