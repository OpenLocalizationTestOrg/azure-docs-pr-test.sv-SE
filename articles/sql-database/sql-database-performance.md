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
# <a name="monitor-and-improve-performance"></a>Övervaka och förbättra prestanda
Azure SQL-databas identifierar potentiella problem i din databas och rekommenderar åtgärder som kan förbättra prestandan för din arbetsbelastning genom att tillhandahålla intelligent prestandajustering åtgärder och rekommendationer.

tooreview ditt databasprestanda, Använd hello **prestanda** panelen på översiktssidan för hello eller gå nedåt för ”stöd för + felsökning” avsnittet:

   ![Prestanda för vyer](./media/sql-database-performance/entries.png)

Du kan använda hello följande sidor i hello ”stöd för + felsökning” avsnittet:


1. [Översikt över prestanda](#performance-overview) toomonitor databasens prestanda. 
2. [Rekommendationer](#performance-recommendations) toofind rekommendationer som kan förbättra prestandan för din arbetsbelastning.
3. [Query Performance Insight](#query-performance-insight) toofind översta resurs förbrukningsfrågorna.
4. [Automatisk justering](#automatic-tuning) toolet Azure SQL Database automatiskt optimera din databas.

## <a name="performance-overview"></a>Översikt över prestanda
Den här vyn innehåller en sammanfattning av databasens prestanda och hjälper dig med prestanda justera och felsökning. 

![Prestanda](./media/sql-database-performance/performance.png)

* Hej **rekommendationer** panelen visar en uppdelning av justera rekommendationer för databasen (de tre översta rekommendationer visas om det finns flera). Klicka på den här panelen tar dig för**[rekommendationer](#performance-recommendations)**. 
* Hej **justera aktiviteten** panelen innehåller en översikt över hello pågående och slutförda justera åtgärder för din databas, vilket ger en snabb överblick över hello historiken för inställning av aktiviteten. Klicka på den här panelen tar dig toohello fullständig justera historikvyn för din databas.
* Hej **automatisk justering** panelen visar hello [automatisk justering configuration](sql-database-automatic-tuning-enable.md) för databasen (justering alternativ som är automatiskt tillämpade tooyour databas). Om du klickar på den här panelen öppnas hello automation konfigurationsdialogruta.
* Hej **databasfrågor** panelen visar hello sammanfattning av hello frågeprestanda för databasen (övergripande DTU användnings- och upp resurs förbrukningsfrågorna). Klicka på den här panelen tar dig för**[Query Performance Insight](#query-performance-insight)**.

## <a name="performance-recommendations"></a>Rekommendationer
Den här sidan innehåller intelligent [justera rekommendationer](sql-database-advisor.md) som kan förbättra dina databasprestanda. följande typer av rekommendationer hello visas på den här sidan:

* Rekommendationer om vilka index toocreate eller ta bort.
* Rekommendationer när schemat problem identifieras i hello-databasen.
* Rekommendationer när frågor kan dra nytta av frågor med parametrar.

![Prestanda](./media/sql-database-performance/recommendations.png)

Du kan också hitta komplett historik över justera åtgärder som vidtogs i hello tidigare.

Lär dig hur toofind apply rekommendationer i [söka efter och tillämpa rekommendationer](sql-database-advisor-portal.md) artikel.

## <a name="automatic-tuning"></a>Automatisk inställning
Azure SQL-databaser kan automatiskt finjustera databasens prestanda genom att använda [rekommendationer](sql-database-advisor.md). toolearn läsa fler [automatisk justering artikel](sql-database-automatic-tuning.md). tooenable, läsa [hur tooenable automatisk justering](sql-database-automatic-tuning-enable.md).

## <a name="query-performance-insight"></a>Query Performance Insight
[Query Performance Insight](sql-database-query-performance.md) kan du toospend mindre tid felsökning databasens prestanda genom att tillhandahålla:

* Djupare inblick i dina databaser resursförbrukning (DTU). 
* hello högsta CPU förbrukningsfrågorna som potentiellt kan anpassas för bättre prestanda. 
* Hej möjlighet toodrill ned hello detaljer om en fråga. 

  ![instrumentpanel för prestanda](./media/sql-database-query-performance/performance.png)

Mer information om den här sidan finns i artikeln hello  **[hur toouse Query Performance Insight](sql-database-query-performance.md)**.

## <a name="additional-resources"></a>Ytterligare resurser
* [Azure SQL Database-prestandaråd för enskilda databaser](sql-database-performance-guidance.md)
* [När en elastisk pool att användas?](sql-database-elastic-pool-guidance.md)

