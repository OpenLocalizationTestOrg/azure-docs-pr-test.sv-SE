---
title: aaaAdding en Fragmentera med elastiska Databasverktyg | Microsoft Docs
description: "Hur ställer toouse Elastic Scale API: er tooadd nya shards tooa Fragmentera."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: 62a349db-bebe-406f-a120-2f1986f2b286
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: f44b59578376d1238b3012a3cb52339978079f0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-a-shard-using-elastic-database-tools"></a>Lägga till en Fragmentera med hjälp av verktyg för elastisk databas
## <a name="tooadd-a-shard-for-a-new-range-or-key"></a>tooadd en Fragmentera för ett nytt intervall eller en nyckel
Program ofta behöver toosimply lägga till nya shards toohandle data som förväntas av nya nycklar eller viktiga områden för en Fragmentera karta som redan finns. Till exempel ett delat program genom att klient-ID kan behöva tooprovision nya Fragmentera för en ny klient eller varje månad delat data måste en ny Fragmentera etableras innan hello början av varje ny månad. 

Om hello nya viktiga värdeintervallet inte redan är en del av en befintlig mappning är det mycket enkelt tooadd hello nya Fragmentera och koppla hello ny nyckel eller ett område toothat Fragmentera. 

### <a name="example--adding-a-shard-and-its-range-tooan-existing-shard-map"></a>Exempel: lägga till en Fragmentera och dess intervallet tooan Fragmentera karta
Det här exemplet använder hello [TryGetShard](https://msdn.microsoft.com/library/azure/dn823929.aspx) hello [CreateShard](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.createshard.aspx), [CreateRangeMapping](https://msdn.microsoft.com/library/azure/dn807221.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeShardMap`1.CreateRangeMapping\(Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.RangeMappingCreationInfo{`0}\)) metoder, och skapar en instans av hello [ShardLocation](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardlocation.shardlocation.aspx#M:Microsoft.Azure.SqlDatabase.ElasticScale.ShardManagement.ShardLocation.) klass. I exemplet hello nedan, en databas med namnet **sample_shard_2** och alla nödvändiga schemaobjekt inuti den har skapats toohold intervallet [300, 400).  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range being added. 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 
        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Create hello mapping and associate it with hello new shard 
    sm.CreateRangeMapping(new RangeMappingCreationInfo<long> 
                            (new Range<long>(300, 400), shard2, MappingStatus.Online)); 


Alternativt kan använda du Powershell toocreate en ny Fragmentera kartan chef. Ett exempel är tillgänglig [här](https://gallery.technet.microsoft.com/scriptcenter/Azure-SQL-DB-Elastic-731883db).

## <a name="tooadd-a-shard-for-an-empty-part-of-an-existing-range"></a>tooadd en Fragmentera för en tom del av ett befintligt intervall
I vissa fall kan du redan mappats en intervallet tooa Fragmentera och delvis fylls det med data, men nu vill kommande data toobe dirigerad tooa olika Fragmentera. Till exempel du Fragmentera per dag dataintervall och redan har allokerats 50 dagar tooa Fragmentera men dag 24 tooland framtida data i en annan Fragmentera. hello elastisk databas [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) kan utföra denna åtgärd, men om dataflyttning inte är nödvändigt (t.ex, data för hello intervall [25, 50), d.v.s., dag 25 inklusiva too50 uteslutande, ännu inte finns) du kan utföra Det här helt med hello Fragmentera kartan Management API: er direkt.

### <a name="example-splitting-a-range-and-assigning-hello-empty-portion-tooa-newly-added-shard"></a>Exempel: dela ett intervall och tilldela hello tom del tooa nyligen tillagda Fragmentera
En databas med namnet ”sample_shard_2” och alla nödvändiga schemaobjekt inuti den har skapats.  

    // sm is a RangeShardMap object.
    // Add a new shard toohold hello range we will move 
    Shard shard2 = null; 

    if (!sm.TryGetShard(new ShardLocation(shardServer, "sample_shard_2"),out shard2)) 
    { 

        shard2 = sm.CreateShard(new ShardLocation(shardServer, "sample_shard_2"));  
    } 

    // Split hello Range holding Key 25 

    sm.SplitMapping(sm.GetMappingForKey(25), 25); 

    // Map new range holding [25-50) toodifferent shard: 
    // first take existing mapping offline 
    sm.MarkMappingOffline(sm.GetMappingForKey(25)); 
    // now map while offline tooa different shard and take online 
    RangeMappingUpdate upd = new RangeMappingUpdate(); 
    upd.Shard = shard2; 
    sm.MarkMappingOnline(sm.UpdateMapping(sm.GetMappingForKey(25), upd)); 

**Viktiga**: Använd den här tekniken bara om du är att hello intervallet för hello uppdateras mappningen är tom.  Kontrollera hello metoderna ovan inte data för hello-intervall som ska flyttas, så det är bäst tooinclude kontrollerar i kod.  Om det finns rader i hello-intervall som ska flyttas, matchar hello faktiska Datadistribution inte hello uppdaterade Fragmentera kartan. Använd hello [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md) tooperform hello åtgärden i stället i dessa fall.  

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

