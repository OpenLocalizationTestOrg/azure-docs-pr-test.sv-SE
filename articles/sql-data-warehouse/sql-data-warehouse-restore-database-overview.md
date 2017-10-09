---
title: aaaRestore ett Azure data warehouse - lokala och geo-redundant | Microsoft Docs
description: "Översikt över hello databasen återställningsalternativ för att återställa en databas i Azure SQL Data Warehouse."
services: sql-data-warehouse
documentationcenter: NA
author: Lakshmi1812
manager: jhubbard
editor: 
ms.assetid: 3e01c65c-6708-4fd7-82f5-4e1b5f61d304
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: backup-restore
ms.date: 10/31/2016
ms.author: lakshmir;barbkess
ms.openlocfilehash: a96b898372b29d420e1416ca93a172ff8af47fc7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sql-data-warehouse-restore"></a>SQL Data Warehouse-återställning
> [!div class="op_single_selector"]
> * [Översikt över][Overview]
> * [Portal][Portal]
> * [PowerShell][PowerShell]
> * [REST][REST]
> 
> 

SQL Data Warehouse har både lokala och geografiska återställningar som en del av dess data warehouse disaster recovery-funktioner. Använd data warehouse säkerhetskopieringar toorestore data warehouse tooa återställningen peka i hello primära region eller använder geo-redundant säkerhetskopieringar toorestore tooa olika geografiska område. Den här artikeln förklarar hello information om du återställer data warehouse.

## <a name="what-is-a-data-warehouse-restore"></a>Vad är en återställning av data warehouse?
En återställning av data warehouse är ett nytt datalager som har skapats från en säkerhetskopia av en befintlig eller borttagna data warehouse. hello återställda data warehouse återskapar hello säkerhetskopierade data warehouse vid en viss tid. Eftersom SQL Data Warehouse är ett distribuerat system, skapas en återställning av data warehouse från många säkerhetskopior som lagras i Azure BLOB. 

Återställning av databasen är en viktig del av alla disaster recovery strategi för affärskontinuitet och eftersom den återskapar dina data efter oavsiktliga skador eller tas bort.

Mer information finns i:

* [SQL Data Warehouse-säkerhetskopieringar](sql-data-warehouse-backups.md)
* [Översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md)

## <a name="data-warehouse-restore-points"></a>Återställningspunkter för data warehouse
SQL Data Warehouse använder Azure Storage Blob ögonblicksbilder toobackup hello primära data warehouse som en fördel med att använda Azure Premium-lagring. Varje ögonblicksbild har en återställningspunkt som representerar hello tid hello ögonblicksbild som är igång. toorestore ett datalager, som du väljer en återställningspunkt och utfärda ett kommando för återställning.  

SQL Data Warehouse återställs alltid hello säkerhetskopiering tooa Nytt datalager. Du kan antingen behålla hello återställda data warehouse och hello aktuella eller ta bort en av dem. Om du vill tooreplace hello aktuella data warehouse med hello återställts datalagret, du kan byta namn på den.

Om du behöver toorestore borttagna eller pausas datalager, kan du [skapa ett supportärende](sql-data-warehouse-get-started-create-support-ticket.md). 

<!-- 
### Can I restore a deleted data warehouse?

Yes, you can restore hello last available restore point.

Yes, for hello next seven calendar days. When you delete a data warehouse, SQL Data Warehouse actually keeps hello data warehouse and its snapshots for seven days just in case you need hello data. After seven days, you won't be able toorestore tooany of hello restore points. -->

## <a name="geo-redundant-restore"></a>GEO-redundant återställning
Du kan återställa din datalager tooany dataområdet stöd för Azure SQL Data Warehouse på din valda prestandanivå. Observera att 9000 och 18000 DWU inte stöds i alla regioner hello förhandsversionen.

> [!NOTE]
> en geo-redundant tooperform återställning som du måste inte har valt att inte den här funktionen.
> 
> 

## <a name="restore-timeline"></a>Återställa tidslinjen
Du kan återställa en databas tooany tillgängliga återställningspunkt inom hello senaste sju dagarna. Ögonblicksbilder Starta var fjärde timme tooeight och är tillgängliga i sju dagar. När en ögonblicksbild är äldre än sju dagar, den upphör att gälla och dess återställningspunkten är inte längre tillgänglig.

## <a name="restore-costs"></a>Återställa kostnader
hello lagring kostnad för hello återställda data warehouse debiteras med hello Azure Premium Storage hastighet. 

Om du pausar ett återställda data warehouse, debiteras du för lagring med hello Azure Premium Storage hastighet. hello fördelen pausa är inte debiteras du för hello DWU datorresurser.

Mer information om priser för SQL Data Warehouse finns [priser för SQL Data Warehouse](https://azure.microsoft.com/pricing/details/sql-data-warehouse/).

## <a name="uses-for-restore"></a>Används för återställning
hello primär användning för återställning av data warehouse är toorecover data efter oavsiktlig dataförlust eller skadade data.

Du kan också använda data warehouse återställning tooretain en säkerhetskopia för längre än sju dagar. När hello säkerhetskopia återställs har hello data warehouse online och kan pausa det under obestämd tid toosave beräkning kostnader. hello pausas databas debiteras lagring med hello Azure Premium Storage hastighet. 

## <a name="related-topics"></a>Relaterade ämnen
### <a name="scenarios"></a>Scenarier
* En översikt över verksamhetskontinuitet, se [översikt över verksamhetskontinuitet](../sql-database/sql-database-business-continuity.md)

<!-- ### Tasks -->

tooperform ett informationslager återställa kan du återställa med hjälp av:

* Azure portal, se [återställa ett data warehouse med hjälp av hello Azure-portalen](sql-data-warehouse-restore-database-portal.md)
* PowerShell-cmdlets finns [återställa ett data warehouse med hjälp av PowerShell-cmdlets](sql-data-warehouse-restore-database-powershell.md)
* REST-API: er, se [återställa ett data warehouse med hjälp av hello REST API: er](sql-data-warehouse-restore-database-rest-api.md)

<!-- ### Tutorials -->

<!--Image references-->

<!--Article references-->
[Azure SQL Database business continuity overview]: ../sql-database/sql-database-business-continuity.md
[Overview]: ./sql-data-warehouse-restore-database-overview.md
[Portal]: ./sql-data-warehouse-restore-database-portal.md
[PowerShell]: ./sql-data-warehouse-restore-database-powershell.md
[REST]: ./sql-data-warehouse-restore-database-rest-api.md

<!--MSDN references-->


<!--Other Web references-->
