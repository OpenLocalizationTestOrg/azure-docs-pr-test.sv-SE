---
title: 'T-SQL: Hantera en Azure SQL Database-elastisk pool | Microsoft Docs'
description: "Använda T-SQL toomanage en Azure SQL Database-elastisk pool."
services: sql-database
documentationcenter: 
author: srinia
manager: jhubbard
editor: 
ms.assetid: 4e288e17-bc3e-4255-9fbe-0a2ac0dbd7dd
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-management
ms.date: 05/27/2016
ms.author: srinia
ms.openlocfilehash: 666f131b2c88849a1a9ea83381bbc27548e93599
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-an-elastic-pool-with-transact-sql"></a>Övervaka och hantera en elastisk pool med Transact-SQL
Det här avsnittet beskrivs hur du toomanage skalbara [elastiska pooler](sql-database-elastic-pool.md) med Transact-SQL.  Du kan också skapa och hantera en Azure elastisk pool hello [Azure-portalen](https://portal.azure.com/), [PowerShell](sql-database-elastic-pool-manage-powershell.md), hello REST API eller [C#](sql-database-elastic-pool-manage-csharp.md). Du kan också skapa och flytta databaser till och från elastiska pooler med [Transact-SQL](sql-database-elastic-pool-manage-tsql.md).


Använd hello [Create Database (Azure SQL Database)](https://msdn.microsoft.com/library/dn268335.aspx) och [Alter Database(Azure SQL Database)](https://msdn.microsoft.com/library/mt574871.aspx) kommandon toocreate och flytta databaser till och från elastiska pooler. hello elastiska poolen måste finnas innan du kan använda dessa kommandon. Dessa kommandon påverkar endast databaser. Skapandet av nya pooler och hello-inställningarna för pool-egenskaper (till exempel min och max edtu: er) kan inte ändras med T-SQL-kommandon.

## <a name="create-a-pooled-database-in-an-elastic-pool"></a>Skapa en delad databas i en elastisk pool
Använd hello CREATE DATABASE-kommandot med hello SERVICE_OBJECTIVE alternativet.   

    CREATE DATABASE db1 ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3M100] ));
    -- Create a database named db1 in an elastic named S3M100.

Alla databaser i en elastisk pool ärver hello tjänstnivån för hello elastiska poolen (Basic, Standard, Premium). 

## <a name="move-a-database-between-elastic-pools"></a>Flytta en databas mellan elastiska pooler
Använd hello ALTER DATABASE-kommandot med hello ändra och ange tjänsten\_mål alternativet som ELASTISK\_POOL. Ange hello namn toohello namnet på hello målpool.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [PM125] ));
    -- Move hello database named db1 tooan elastic named P1M125  

## <a name="move-a-database-into-an-elastic-pool"></a>Flytta en databas till en elastisk pool
Använd hello ALTER DATABASE-kommandot med hello ändra och ange tjänsten\_mål alternativet som ELASTIC_POOL. Ange hello namn toohello namnet på hello målpool.

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = ELASTIC_POOL (name = [S3100] ));
    -- Move hello database named db1 tooan elastic named S3100.

## <a name="move-a-database-out-of-an-elastic-pool"></a>Flytta en databas från en elastisk pool
Använda hello ALTER DATABASE-kommandot och ange hello SERVICE_OBJECTIVE tooone av hello prestandanivåer (till exempel S0 eller S1).

    ALTER DATABASE db1 MODIFY ( SERVICE_OBJECTIVE = 'S1');
    -- Changes hello database into a stand-alone database with hello service objective S1.

## <a name="list-databases-in-an-elastic-pool"></a>Lista över databaser i en elastisk pool
Använd hello [sys.database\_service \_mål visa](https://msdn.microsoft.com/library/mt712619) toolist alla hello databaser i en elastisk pool. Logga in toohello master databasvyn tooquery hello.

    SELECT d.name, slo.*  
    FROM sys.databases d 
    JOIN sys.database_service_objectives slo  
    ON d.database_id = slo.database_id
    WHERE elastic_pool_name = 'MyElasticPool'; 

## <a name="get-resource-usage-data-for-an-elastic-pool"></a>Hämta resursanvändningsdata för en elastisk pool
Använd hello [sys.elastic\_pool \_resurs \_stats visa](https://msdn.microsoft.com/library/mt280062.aspx) tooexamine hello resurs användningsstatistik för en elastisk pool på en logisk server. Logga in toohello master databasvyn tooquery hello.

    SELECT * FROM sys.elastic_pool_resource_stats 
    WHERE elastic_pool_name = 'MyElasticPool'
    ORDER BY end_time DESC;

## <a name="get-resource-usage-for-a-pooled-database"></a>Hämta resursanvändningen för en delad databas
Använd hello [sys.dm\_ db\_ resurs\_stats visa](https://msdn.microsoft.com/library/dn800981.aspx) eller [sys.resource \_stats visa](https://msdn.microsoft.com/library/dn269979.aspx) tooexamine hello resurs användningsstatistik för en databas i en elastisk pool. Den här processen är liknande tooquerying resursanvändningen för en enskild databas.

## <a name="next-steps"></a>Nästa steg
Du kan hantera elastiska databaser i poolen hello genom att skapa elastiska jobb när du har skapat en elastisk pool. Elastiska jobb underlätta köra T-SQL-skript mot valfritt antal databaser i hello pool. Mer information finns i [elastisk databas översikt över](sql-database-elastic-jobs-overview.md). 

Se [skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md): Använd elastisk databas verktyg tooscale ut, flytta data, fråga eller skapa transaktioner.

