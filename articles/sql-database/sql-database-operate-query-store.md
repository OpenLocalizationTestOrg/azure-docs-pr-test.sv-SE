---
title: aaaOperating Query Store i Azure SQL Database
description: "Lär dig hur toooperate hello Query Store i Azure SQL Database"
keywords: 
services: sql-database
documentationcenter: 
author: bonova
manager: jhubbard
editor: 
ms.assetid: 0cccf6bd-1327-44f7-a6f9-8eff0c210463
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: sqldb-performance
ms.workload: data-management
ms.date: 11/08/2016
ms.author: bonova
ms.openlocfilehash: ab741e49685bf6df3bceab219aaf57ea51cdb25d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="operating-hello-query-store-in-azure-sql-database"></a>Operativ hello Query Store i Azure SQL Database
Query Store i Azure är en helt hanterad databasfunktion som samlar in kontinuerligt och Detaljerad historisk information om alla frågor. Du kan se om Query Store som liknande tooan flygplan svarta data lådan som avsevärt förenklar frågeprestanda felsökning av både för molnet och lokala kunder. Den här artikeln förklarar vissa aspekter av operativsystem Query Store i Azure. Med den här före insamlade fråga efter data snabbt diagnostisera och lösa problem med prestanda och därmed ägna mer tid åt att fokusera på verksamheten. 

Query Store har [globalt tillgänglig](https://azure.microsoft.com/updates/general-availability-azure-sql-database-query-store/) i Azure SQL Database sedan November 2015. Query Store är hello grunden för analys av minnesprestanda och finjustera funktioner, t.ex [SQL Database Advisor och prestanda instrumentpanel](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Hello närvarande för publicering av den här artikeln, körs Query Store i mer än 200 000 databaserna i Azure, insamling av frågan-relaterad information för flera månader utan avbrott.

> [!IMPORTANT]
> Microsoft är i hello att aktivera Query Store för alla Azure SQL-databaser (befintliga och nya). 
> 
> 

## <a name="optimal-query-store-configuration"></a>Optimal Query Store-konfiguration
Det här avsnittet beskrivs bästa konfigurationen standardvärden som är utformad tooensure tillförlitliga driften av hello Query Store och beroende funktioner, till exempel [SQL Database Advisor och prestanda instrumentpanel](https://azure.microsoft.com/updates/sqldatabaseadvisorga/). Standardkonfigurationen är optimerad för kontinuerlig datainsamling som är minimal tid i OFF/READ_ONLY tillstånd.

| Konfiguration | Beskrivning | Standard | Kommentar |
| --- | --- | --- | --- |
| MAX_STORAGE_SIZE_MB |Anger hello gränsen för hello datautrymme Query Store kan göra i z kunddatabasen |100 |För nya databaser |
| INTERVAL_LENGTH_MINUTES |Definierar storleken på tidsperioden under vilken insamlade körningsstatistik för frågeplaner sammanställs och beständig. Alla aktiva frågeplan innehåller högst en rad under en tidsperiod som definierats med den här konfigurationen |60 |För nya databaser |
| STALE_QUERY_THRESHOLD_DAYS |Tidsbaserade Rensa princip som styr hello kvarhållningsperiod beständiga körningsstatistik och inaktiva frågor |30 |För nya databaser och databaser med föregående standard (367) |
| SIZE_BASED_CLEANUP_MODE |Anger om automatisk Datarensning sker när Query Store datastorleken närmar sig hello gränsen |AUTOMATISK |För alla databaser |
| QUERY_CAPTURE_MODE |Anger om alla frågor eller endast en delmängd av frågor spåras |AUTOMATISK |För alla databaser |
| FLUSH_INTERVAL_SECONDS |Anger maximal period under vilken avbildade körningsstatistik lagras i minnet, innan toodisk |900 |För nya databaser |
|  | | | |

> [!IMPORTANT]
> Dessa standardvärden används automatiskt i hello sista steget i Query Store aktivering i alla Azure SQL-databaser (se föregående viktigt). Efter den här ljus upp ändrar Azure SQL Database inte konfigurationsvärden som angetts av kunder, om inte de påverkas negativt primära arbetsbelastning eller tillförlitlig drift av hello Query Store.
> 
> 

Om du vill toostay med dina egna inställningar använder [ALTER DATABASE med alternativ för Query Store](https://msdn.microsoft.com/library/bb522682.aspx) toorevert configuration toohello tidigare tillstånd. Checka ut [metodtips med hello Query Store](https://msdn.microsoft.com/library/mt604821.aspx) i ordning toolearn hur översta väljer optimala konfigurationsparametrar.

## <a name="next-steps"></a>Nästa steg
[SQL-databas Performance Insight](sql-database-performance.md)

## <a name="additional-resources"></a>Ytterligare resurser
För mer information utcheckningen hello följande artiklar:

* [En svarta lådan för data för databasen](https://azure.microsoft.com/blog/query-store-a-flight-data-recorder-for-your-database) 
* [Övervakning av prestanda genom att använda hello Query Store](https://msdn.microsoft.com/library/dn817826.aspx)
* [Query Store Användningsscenarier](https://msdn.microsoft.com/library/mt614796.aspx)
* [Övervakning av prestanda genom att använda hello Query Store](https://msdn.microsoft.com/library/dn817826.aspx) 

