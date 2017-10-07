---
title: aaaMonitoring & prestandajustering - Azure SQL Database | Microsoft Docs
description: "Tips för prestandajustering i Azure SQL Database via utvärdera och förbättra."
services: sql-database
documentationcenter: 
author: v-shysun
manager: felixwu
editor: 
keywords: "prestandajustering för SQL, prestandajustering för databasen, sql prestandajustering tips, prestandajustering för sql-databas"
ms.assetid: eb7b3f66-3b33-4e1b-84fb-424a928a6672
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: v-shysun
ms.openlocfilehash: 9e196831902aa6ea841ef14caf5713e82ebfc62d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitoring-and-performance-tuning"></a>Övervaka och justera prestanda

Azure SQL Database hanteras automatiskt och flexibel datatjänst där du kan lätt övervaka användningen, lägga till eller ta bort resurser (processor, minne, i/o), hitta rekommendationer som kan förbättra prestandan för din databas eller låt databasen anpassa tooyour arbetsbelastning och automatiskt optimera prestanda.

Den här artikeln innehåller en översikt över övervakning och prestandajustering alternativ som är tillgängliga i Azure SQL Database.

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="monitoring-and-troubleshooting-database-performance"></a>Övervakning och felsökning databasprestanda

Azure SQL Database aktiverar du tooeasily övervaka din databasanvändningen och identifiera frågor som kan orsaka prestandaproblem hello. Du kan övervaka databasprestanda med hjälp av Azure portal eller system vyer. Du har följande alternativ för övervakning och felsökning databasprestanda hello:

1. I hello [Azure-portalen](https://portal.azure.com), klickar du på **SQL-databaser**hello databasen och välj sedan använda hello övervakning diagram toolook för resurser som närmar sig sin högsta. DTU-förbrukning visas som standard. Klicka på **redigera** toochange hello tidsintervall och värden som visas.
2. Använd [Query Performance Insight](sql-database-query-performance.md) tooidentify hello frågor som tillbringar hello mest resurser.
3. Du kan använda dynamiska hanteringsvyer (av DMV: er), Extended Events (`XEvents`), och hello Query Store i SSMS tooget prestandaparametrar i realtid.

Se hello [prestanda vägledning avsnittet](sql-database-performance-guidance.md) toofind tekniker som du kan använda tooimprove prestanda i Azure SQL Database om du identifierar vissa problem med att använda dessa rapporter eller vyer.

> [!IMPORTANT] 
> Det rekommenderas att du alltid använder hello senaste versionen av Management Studio tooremain synkroniseras med uppdateringar tooMicrosoft Azure och SQL-databas. [Uppdatera SQL Server Management Studio](https://msdn.microsoft.com/library/mt238290.aspx).
>

## <a name="optimize-database-tooimprove-performance"></a>Optimera databasprestanda för tooimprove

Azure SQL-databas kan du tooidentify affärsmöjligheter tooimprove och optimera frågeprestanda utan att ändra resurser genom att granska [rekommendationer för prestandajustering](sql-database-advisor.md). Index som saknas och dåligt optimerade frågor är vanliga orsaker till dåliga databasprestanda. Du kan använda dessa prestandajustering rekommendationer tooimprove prestandan för din arbetsbelastning.
Du kan också låta Azure SQL-databas för[automatiskt optimera prestanda för dina frågor](sql-database-automatic-tuning.md) genom att använda alla identifierade rekommendationer och verifiering av att de förbättras databasens prestanda. Du kan använda följande alternativ tooimprove prestandan för din databas hello:

1. Använd [SQL Database Advisor](sql-database-advisor-portal.md) tooview rekommendationer för att skapa och släppa index, Parameterisera frågor och åtgärda problem med schemat.
2. [Aktivera automatisk justering](sql-database-automatic-tuning-enable.md) och låta Azure SQL-databasen automatiskt korrigera identifieras prestandaproblem.

## <a name="improving-database-performance-with-more-resources"></a>Förbättrad databasprestanda med mer resurser

Slutligen, om det finns ingen tillämplig objekt som kan förbättra prestandan för din databas, kan du ändra hello mängden tillgängliga resurser i Azure SQL Database. Du kan tilldela fler resurser genom att ändra hello [tjänstnivån](sql-database-service-tiers.md) av en fristående databas eller öka hello edtu: er för en elastisk pool när som helst.
1. För fristående databaser, kan du [ändra tjänstnivå](sql-database-service-tiers.md) på begäran tooimprove databasens prestanda.
2. Överväg att använda för flera databaser [elastiska pooler](sql-database-elastic-pool-guidance.md) tooscale resurser automatiskt.

## <a name="tune-and-refactor-application-or-database-code"></a>Finjustera och flytta program eller databas

Du kan ändra program kod toomore optimalt använder hello-databas, ändra index, tvinga planer eller använda tips toomanually anpassa hello databasen tooyour arbetsbelastning. Hitta vissa råd och tips för manuell justering och skriva om hello koden i hello [prestanda vägledning avsnittet](sql-database-performance-guidance.md) artikel.


## <a name="next-steps"></a>Nästa steg

- tooenable automatisk justering i Azure SQL Database och gör att automatisk justering funktion fullständigt hantera din arbetsbelastning, se [aktivera automatisk justering](sql-database-automatic-tuning-enable.md).
- toouse manuell inställning, kan du granska [justera rekommendationerna i Azure-portalen](sql-database-advisor-portal.md) och tillämpa manuellt hello som förbättra prestanda för dina frågor.
- Ändra resurser som är tillgängliga i databasen genom att ändra [Azure SQL Database servicenivåer](sql-database-performance-guidance.md)