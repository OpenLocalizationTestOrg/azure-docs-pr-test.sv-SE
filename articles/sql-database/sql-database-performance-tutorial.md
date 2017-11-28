---
title: "aaaTroubleshoot prestanda utfärdar och optimera din databas | Microsoft Docs"
description: "Tillämpa prestanda rekommendationer tooyour SQL-databas som ensa hur toogain insikter om hello prestanda för hello frågor körs mot databasen"
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
ms.openlocfilehash: e948d30ac74eecf45420d5d77ef55e3c0b6f3f47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a><span data-ttu-id="97756-103">Felsöka problem med prestanda och optimera din databas</span><span class="sxs-lookup"><span data-stu-id="97756-103">Troubleshoot performance issues and optimize your database</span></span>

<span data-ttu-id="97756-104">Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="97756-104">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="97756-105">I den här kursen lär du dig:</span><span class="sxs-lookup"><span data-stu-id="97756-105">In this tutorial you learn to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="97756-106">Granska, tillämpa och återställa improvement rekommendationer</span><span class="sxs-lookup"><span data-stu-id="97756-106">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="97756-107">Hitta frågor med hög resursutnyttjande</span><span class="sxs-lookup"><span data-stu-id="97756-107">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="97756-108">Hitta tidskrävande frågor</span><span class="sxs-lookup"><span data-stu-id="97756-108">Find long running queries</span></span>

> <span data-ttu-id="97756-109">Du behöver en kontinuerlig arbetsbelastning på en databas med prestandaproblem – saknas ett index till exempel tooreceive en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="97756-109">You need a continuous workload on a database with performance issues – missing an index for example tooreceive a recommendation.</span></span>
>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="97756-110">Logga in toohello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="97756-110">Log in toohello Azure portal</span></span>

<span data-ttu-id="97756-111">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="97756-111">Log in toohello [Azure portal](https://portal.azure.com/).</span></span>

## <a name="review-and-apply-a-recommendation"></a><span data-ttu-id="97756-112">Granska och Använd en rekommendation</span><span class="sxs-lookup"><span data-stu-id="97756-112">Review and apply a recommendation</span></span>

<span data-ttu-id="97756-113">Följ dessa steg tooapply en rekommendation från hello system för databasen:</span><span class="sxs-lookup"><span data-stu-id="97756-113">Follow these steps tooapply a recommendation from hello system for your database:</span></span>

1. <span data-ttu-id="97756-114">Klicka på hello **rekommendationer** -menyn i hello databasbladet.</span><span class="sxs-lookup"><span data-stu-id="97756-114">Click hello **Performance recommendations** menu in hello database blade.</span></span>

    ![rekommendation för prestanda](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. <span data-ttu-id="97756-116">Välj en aktiv rekommendation hello listan över rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="97756-116">From hello list of recommendations, select an active recommendation.</span></span> <span data-ttu-id="97756-117">I det här exemplet skapar Index.</span><span class="sxs-lookup"><span data-stu-id="97756-117">In this example, Create Index.</span></span>

    ![Välj rekommendation](./media/sql-database-performance-tutorial/create_index.png)

3. <span data-ttu-id="97756-119">Tillämpa hello rekommendationen genom att klicka på hello **tillämpa** knappen.</span><span class="sxs-lookup"><span data-stu-id="97756-119">Apply hello recommendation by clicking hello **Apply** button.</span></span> <span data-ttu-id="97756-120">Du kan också granska hello rekommendation information och se hello T-SQL-skript för att köra genom att klicka på **Visa skript** knappen.</span><span class="sxs-lookup"><span data-stu-id="97756-120">Optionally, review hello recommendation details and see hello T-SQL script too be executed by clicking on **View Script** button.</span></span>

    ![Tillämpa rekommendation](./media/sql-database-performance-tutorial/apply.png)

4. <span data-ttu-id="97756-122">[Valfritt] Aktivera automatisk justering för rekommendationer toobe tillämpas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="97756-122">[Optional] Enable automatic tuning for recommendations toobe applied automatically.</span></span>

    ![automatisk justering](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a><span data-ttu-id="97756-124">Återställa en rekommendation</span><span class="sxs-lookup"><span data-stu-id="97756-124">Revert a recommendation</span></span>

<span data-ttu-id="97756-125">hello Database Advisor övervakar varje rekommendation implementeras.</span><span class="sxs-lookup"><span data-stu-id="97756-125">hello Database Advisor monitors every recommendation implemented.</span></span> <span data-ttu-id="97756-126">Om en rekommendation inte förbättrar hello arbetsbelastning som den kommer att återställas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="97756-126">If a recommendation doesn't improve hello workload it will be automatically reverted.</span></span> <span data-ttu-id="97756-127">Manuellt återställa en rekommendation är möjligt, men inte behövs i de flesta fall.</span><span class="sxs-lookup"><span data-stu-id="97756-127">Manually reverting a recommendation is possible, but not necessary in most cases.</span></span> <span data-ttu-id="97756-128">toorevert en rekommendation:</span><span class="sxs-lookup"><span data-stu-id="97756-128">toorevert a recommendation:</span></span>

1. <span data-ttu-id="97756-129">Gå toohello prestanda rekommendationer menyn och välj en av hello tillämpas rekommendationer.</span><span class="sxs-lookup"><span data-stu-id="97756-129">Go toohello performance recommendations menu and select one of hello applied recommendations.</span></span>

    ![Välj rekommendation](./media/sql-database-performance-tutorial/select.png)

2. <span data-ttu-id="97756-131">Klicka i vyn Detaljer hello **Återställ**.</span><span class="sxs-lookup"><span data-stu-id="97756-131">In hello details view, click **Revert**.</span></span>

    ![återställa rekommendation](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-hello-query-that-consumes-hello-most-resources"></a><span data-ttu-id="97756-133">Hitta hello-frågan som förbrukar hello mest resurser</span><span class="sxs-lookup"><span data-stu-id="97756-133">Find hello query that consumes hello most resources</span></span>

<span data-ttu-id="97756-134">Följ dessa steg toofind hello-fråga som förbrukar hello mest resurser:</span><span class="sxs-lookup"><span data-stu-id="97756-134">Follow these steps toofind hello query consuming hello most resources:</span></span>

1. <span data-ttu-id="97756-135">Klicka på hello **Query Performance Insight** -menyn i hello databasbladet.</span><span class="sxs-lookup"><span data-stu-id="97756-135">Click on hello **Query Performance Insight** menu in hello database blade.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. <span data-ttu-id="97756-137">Välj en resurstyp.</span><span class="sxs-lookup"><span data-stu-id="97756-137">Select a resource type.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/select_resource_type.png)

3. <span data-ttu-id="97756-139">Välj hello första frågan i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="97756-139">Select hello first query in hello table.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/select_query.png)

4. <span data-ttu-id="97756-141">Granska hello Frågedetaljer.</span><span class="sxs-lookup"><span data-stu-id="97756-141">Review hello query details.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-hello-longest-running-query"></a><span data-ttu-id="97756-143">Hitta hello längsta kör frågan</span><span class="sxs-lookup"><span data-stu-id="97756-143">Find hello longest running query</span></span>

1. <span data-ttu-id="97756-144">Gå tooQuery Performance Insight och välj hello **tidskrävande frågor** fliken.</span><span class="sxs-lookup"><span data-stu-id="97756-144">Go tooQuery Performance Insight and select hello **Long running queries** tab.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/long_running.png)

3. <span data-ttu-id="97756-146">Välj hello första frågan i hello tabell.</span><span class="sxs-lookup"><span data-stu-id="97756-146">Select hello first query in hello table.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/select_first_query.png)

4. <span data-ttu-id="97756-148">Granska hello Frågedetaljer.</span><span class="sxs-lookup"><span data-stu-id="97756-148">Review hello query details.</span></span>

    ![frågan insikter](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a><span data-ttu-id="97756-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="97756-150">Next steps</span></span> 
<span data-ttu-id="97756-151">Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda.</span><span class="sxs-lookup"><span data-stu-id="97756-151">Missing indexes and poorly optimized queries are common reasons for poor database performance.</span></span> <span data-ttu-id="97756-152">I den här kursen har du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="97756-152">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="97756-153">Granska, tillämpa och återställa improvement rekommendationer</span><span class="sxs-lookup"><span data-stu-id="97756-153">Review, apply and revert performance improvement recommendations</span></span>
> * <span data-ttu-id="97756-154">Hitta frågor med hög resursutnyttjande</span><span class="sxs-lookup"><span data-stu-id="97756-154">Find queries with high resource utilization</span></span>
> * <span data-ttu-id="97756-155">Hitta tidskrävande frågor</span><span class="sxs-lookup"><span data-stu-id="97756-155">Find long running queries</span></span>

[<span data-ttu-id="97756-156">SQL Database prestanda över tips</span><span class="sxs-lookup"><span data-stu-id="97756-156">SQL Database performance tuning tips</span></span>](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
