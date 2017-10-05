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
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a>Felsöka problem med prestanda och optimera din databas

Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda. I den här kursen lär du dig:
> [!div class="checklist"]
> * Granska, tillämpa och återställa improvement rekommendationer
> * Hitta frågor med hög resursutnyttjande
> * Hitta tidskrävande frågor

> Du behöver en kontinuerlig arbetsbelastning på en databas med prestandaproblem – saknas ett index till exempel för att få en rekommendation.
>

## <a name="log-in-to-the-azure-portal"></a>Logga in på Azure Portal

Logga in på [Azure-portalen](https://portal.azure.com/).

## <a name="review-and-apply-a-recommendation"></a>Granska och Använd en rekommendation

Följ dessa steg om du vill använda en rekommendation från systemet för databasen:

1. Klicka på den **rekommendationer** -menyn i databasbladet.

    ![rekommendation för prestanda](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. Välj en aktiv rekommendation från listan över rekommendationer. I det här exemplet skapar Index.

    ![Välj rekommendation](./media/sql-database-performance-tutorial/create_index.png)

3. Använda rekommendationen genom att klicka på den **tillämpa** knappen. Du kan också granska information om en rekommendation och se T-SQL-skript för att köras genom att klicka på **Visa skript** knappen.

    ![Tillämpa rekommendation](./media/sql-database-performance-tutorial/apply.png)

4. [Valfritt] Aktivera automatisk justering för rekommendationer tillämpas automatiskt.

    ![automatisk justering](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a>Återställa en rekommendation

Database Advisor övervakar varje rekommendation implementeras. Om en rekommendation inte förbättrar arbetsbelastningen som den kommer att återställas automatiskt. Manuellt återställa en rekommendation är möjligt, men inte behövs i de flesta fall. Återställa en rekommendation:

1. Gå till menyn prestanda rekommendationer och välj något av rekommendationerna som används.

    ![Välj rekommendation](./media/sql-database-performance-tutorial/select.png)

2. Klicka i Detaljvyn **Återställ**.

    ![återställa rekommendation](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-the-query-that-consumes-the-most-resources"></a>Leta upp frågan som förbrukar mest resurser

Följ dessa steg om du vill leta upp frågan som förbrukar mest resurser:

1. Klicka på den **Query Performance Insight** -menyn i databasbladet.

    ![frågan insikter](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. Välj en resurstyp.

    ![frågan insikter](./media/sql-database-performance-tutorial/select_resource_type.png)

3. Välj den första frågan i tabellen.

    ![frågan insikter](./media/sql-database-performance-tutorial/select_query.png)

4. Granska informationen för frågan.

    ![frågan insikter](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-the-longest-running-query"></a>Leta upp längst körningstid frågan

1. Gå till Query Performance Insight och välj den **tidskrävande frågor** fliken.

    ![frågan insikter](./media/sql-database-performance-tutorial/long_running.png)

3. Välj den första frågan i tabellen.

    ![frågan insikter](./media/sql-database-performance-tutorial/select_first_query.png)

4. Granska informationen för frågan.

    ![frågan insikter](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a>Nästa steg 
Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda. I den här kursen har du lärt dig att:
> [!div class="checklist"]
> * Granska, tillämpa och återställa improvement rekommendationer
> * Hitta frågor med hög resursutnyttjande
> * Hitta tidskrävande frågor

[SQL Database prestanda över tips](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
