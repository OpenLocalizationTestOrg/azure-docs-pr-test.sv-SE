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
# <a name="troubleshoot-performance-issues-and-optimize-your-database"></a>Felsöka problem med prestanda och optimera din databas

Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda. I den här kursen lär du dig:
> [!div class="checklist"]
> * Granska, tillämpa och återställa improvement rekommendationer
> * Hitta frågor med hög resursutnyttjande
> * Hitta tidskrävande frågor

> Du behöver en kontinuerlig arbetsbelastning på en databas med prestandaproblem – saknas ett index till exempel tooreceive en rekommendation.
>

## <a name="log-in-toohello-azure-portal"></a>Logga in toohello Azure-portalen

Logga in toohello [Azure-portalen](https://portal.azure.com/).

## <a name="review-and-apply-a-recommendation"></a>Granska och Använd en rekommendation

Följ dessa steg tooapply en rekommendation från hello system för databasen:

1. Klicka på hello **rekommendationer** -menyn i hello databasbladet.

    ![rekommendation för prestanda](./media/sql-database-performance-tutorial/perf_recommendations.png)

2. Välj en aktiv rekommendation hello listan över rekommendationer. I det här exemplet skapar Index.

    ![Välj rekommendation](./media/sql-database-performance-tutorial/create_index.png)

3. Tillämpa hello rekommendationen genom att klicka på hello **tillämpa** knappen. Du kan också granska hello rekommendation information och se hello T-SQL-skript för att köra genom att klicka på **Visa skript** knappen.

    ![Tillämpa rekommendation](./media/sql-database-performance-tutorial/apply.png)

4. [Valfritt] Aktivera automatisk justering för rekommendationer toobe tillämpas automatiskt.

    ![automatisk justering](./media/sql-database-performance-tutorial/auto_tuning.png)

## <a name="revert-a-recommendation"></a>Återställa en rekommendation

hello Database Advisor övervakar varje rekommendation implementeras. Om en rekommendation inte förbättrar hello arbetsbelastning som den kommer att återställas automatiskt. Manuellt återställa en rekommendation är möjligt, men inte behövs i de flesta fall. toorevert en rekommendation:

1. Gå toohello prestanda rekommendationer menyn och välj en av hello tillämpas rekommendationer.

    ![Välj rekommendation](./media/sql-database-performance-tutorial/select.png)

2. Klicka i vyn Detaljer hello **Återställ**.

    ![återställa rekommendation](./media/sql-database-performance-tutorial/revert.png)

## <a name="find-hello-query-that-consumes-hello-most-resources"></a>Hitta hello-frågan som förbrukar hello mest resurser

Följ dessa steg toofind hello-fråga som förbrukar hello mest resurser:

1. Klicka på hello **Query Performance Insight** -menyn i hello databasbladet.

    ![frågan insikter](./media/sql-database-performance-tutorial/query_perf_insights.png)

2. Välj en resurstyp.

    ![frågan insikter](./media/sql-database-performance-tutorial/select_resource_type.png)

3. Välj hello första frågan i hello tabell.

    ![frågan insikter](./media/sql-database-performance-tutorial/select_query.png)

4. Granska hello Frågedetaljer.

    ![frågan insikter](./media/sql-database-performance-tutorial/query_details.png)

## <a name="find-hello-longest-running-query"></a>Hitta hello längsta kör frågan

1. Gå tooQuery Performance Insight och välj hello **tidskrävande frågor** fliken.

    ![frågan insikter](./media/sql-database-performance-tutorial/long_running.png)

3. Välj hello första frågan i hello tabell.

    ![frågan insikter](./media/sql-database-performance-tutorial/select_first_query.png)

4. Granska hello Frågedetaljer.

    ![frågan insikter](./media/sql-database-performance-tutorial/review_query_details.png)



## <a name="next-steps"></a>Nästa steg 
Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda. I den här kursen har du lärt dig att:
> [!div class="checklist"]
> * Granska, tillämpa och återställa improvement rekommendationer
> * Hitta frågor med hög resursutnyttjande
> * Hitta tidskrävande frågor

[SQL Database prestanda över tips](https://docs.microsoft.com/azure/sql-database/sql-database-troubleshoot-performance)
