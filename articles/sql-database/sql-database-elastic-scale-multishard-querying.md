---
title: aaaQuery delat Azure SQL-databaser | Microsoft Docs
description: "Köra frågor över delar med hello klientbibliotek för elastisk databas."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: a4379c15-f213-4026-ab6f-a450ee9d5758
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2016
ms.author: torsteng
ms.openlocfilehash: a1f0763935a6807b74aa9dec477714e8d117417d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multi-shard-querying"></a>Flera Fragmentera frågor
## <a name="overview"></a>Översikt
Med hello [elastisk Databasverktyg](sql-database-elastic-scale-introduction.md), kan du skapa delat databaslösningar. **Flera Fragmentera frågar** används för aktiviteter som samling/rapportering av användningsdata som kräver att du kör en fråga som sträcker sig över flera delar. (Skiljer sig från detta för[data beroende routning](sql-database-elastic-scale-data-dependent-routing.md), som utför alla arbete på en enda Fragmentera.) 

1. Hämta en [ **RangeShardMap** ](https://msdn.microsoft.com/library/azure/dn807318.aspx) eller [ **ListShardMap** ](https://msdn.microsoft.com/library/azure/dn807370.aspx) med hello [ **TryGetRangeShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetrangeshardmap.aspx), hello [ **TryGetListShardMap**](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.trygetlistshardmap.aspx), eller hello [ **GetShardMap** ](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.getshardmap.aspx) metod. Se [ **konstruera en ShardMapManager** ](sql-database-elastic-scale-shard-map-management.md#constructing-a-shardmapmanager) och [ **får en RangeShardMap eller ListShardMap**](sql-database-elastic-scale-shard-map-management.md#get-a-rangeshardmap-or-listshardmap).
2. Skapa en  **[MultiShardConnection](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardconnection.aspx)**  objekt.
3. Skapa en  **[MultiShardCommand](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.aspx)**. 
4. Ange hello  **[egenskapen CommandText](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.commandtext.aspx#P:Microsoft.Azure.SqlDatabase.ElasticScale.Query.MultiShardCommand.CommandText)**  tooa T-SQL-kommandot.
5. Köra hello kommandot genom att anropa hello  **[ExecuteReader metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardcommand.executereader.aspx)**.
6. Visa hello resultat med hjälp av hello  **[MultiShardDataReader klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multisharddatareader.aspx)**. 

## <a name="example"></a>Exempel
hello följande kod visar hello användning av flera Fragmentera fråga med hjälp av en viss **ShardMap** med namnet *myShardMap*. 

    using (MultiShardConnection conn = new MultiShardConnection( 
                                        myShardMap.GetShards(), 
                                        myShardConnectionString) 
          ) 
    { 
    using (MultiShardCommand cmd = conn.CreateCommand())
           { 
            cmd.CommandText = "SELECT c1, c2, c3 FROM ShardedTable"; 
            cmd.CommandType = CommandType.Text; 
            cmd.ExecutionOptions = MultiShardExecutionOptions.IncludeShardNameColumn; 
            cmd.ExecutionPolicy = MultiShardExecutionPolicy.PartialResults; 

            using (MultiShardDataReader sdr = cmd.ExecuteReader()) 
                { 
                    while (sdr.Read())
                        { 
                            var c1Field = sdr.GetString(0); 
                            var c2Field = sdr.GetFieldValue<int>(1); 
                            var c3Field = sdr.GetFieldValue<Int64>(2);
                        } 
                 } 
           } 
    } 


En viktigaste skillnaden är hello konstruktionen av flera Fragmentera anslutningar. Där **SqlConnection** körs på en enskild databas hello **MultiShardConnection** tar en ***samling shards*** som indata. Fyll i hello samling shards från en Fragmentera karta. hello-frågan körs sedan på hello samling shards med **UNION ALL** semantik tooassemble ett övergripande resultat. Du kan också hello namnet på hello Fragmentera där hello raden härstammar från kan läggas till toohello utdata med hello **ExecutionOptions** -egenskapen i kommandot. 

Observera hello anrop för**myShardMap.GetShards()**. Den här metoden hämtar alla delar i hello Fragmentera mappning och ger ett enkelt sätt toorun en fråga över alla relevanta databaser. hello samling shards för en fråga med flera Fragmentera kan anpassas ytterligare genom att utföra en LINQ fråga över hello datainsamling som returnerades från hello anrop för**myShardMap.GetShards()**. I kombination med hello ofullständiga resultat princip har hello aktuella kapaciteten i flera Fragmentera frågar utformad toowork bra för flera in toohundreds av shards.

En begränsning med flera Fragmentera frågar är för närvarande hello bristande verifieringen av delar och shardlets som efterfrågas. Data dependent routning verifierar att en given Fragmentera tillhör hello Fragmentera kartan för närvarande hello frågor till, men flera Fragmentera frågor utför inte den här kontrollen. Detta kan leda toomulti Fragmentera frågor som körs på databaser som har tagits bort från hello Fragmentera kartan.

## <a name="multi-shard-queries-and-split-merge-operations"></a>Flera Fragmentera frågor och dela merge-operationer
Flera Fragmentera frågor bekräftar inte om shardlets på hello databas deltar i pågående dela merge-åtgärder. (Se [skalning verktyget hello elastisk databas dela sammanslagna](sql-database-elastic-scale-overview-split-and-merge.md).) Detta kan leda tooinconsistencies där rader från hello samma shardlet visa för flera databaser i hello samma flera Fragmentera fråga. Tänk på att dessa begränsningar och Överväg tömmer pågående delade dokument och förändringar toohello Fragmentera kartan när du utför flera Fragmentera frågor.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

## <a name="see-also"></a>Se även
**[System.Data.SqlClient](http://msdn.microsoft.com/library/System.Data.SqlClient.aspx)**  klasser och metoder.

Hantera shards med hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md). Innehåller ett namnområde som kallas [Microsoft.Azure.SqlDatabase.ElasticScale.Query](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.aspx) som ger hello möjlighet tooquery flera delar med hjälp av en enskild fråga och resultat. Det ger en frågar abstraktion över en mängd shards. Det ger också alternativa körningsprinciper, i synnerhet ofullständiga resultat, toodeal med fel vid fråga över flera delar.  

