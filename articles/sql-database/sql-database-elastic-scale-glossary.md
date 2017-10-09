---
title: aaaElastic databasen verktyg ordlista | Microsoft Docs
description: "Förklaring av termer som används för elastiska Databasverktyg"
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: a23a4e81-6706-452d-afc1-a550e5e47af9
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: d6573aad9a097e07135b0a64d1dafec19bb8cc7c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-glossary"></a>Ordlista för verktyg för elastisk databas
hello följande villkor definieras för hello [elastisk Databasverktyg](sql-database-elastic-scale-introduction.md), en funktion i Azure SQL Database. hello verktyg är används toomanage [Fragmentera mappar](sql-database-elastic-scale-shard-map-management.md), och inkludera hello [klientbiblioteket](sql-database-elastic-database-client-library.md), hello [för delade sökvägssammanslagning](sql-database-elastic-scale-overview-split-and-merge.md), [elastiska pooler](sql-database-elastic-pool.md), och [frågor](sql-database-elastic-query-overview.md). 

Dessa villkor används i [att lägga till en Fragmentera med elastiska Databasverktyg](sql-database-elastic-scale-add-a-shard.md) och [med hello RecoveryManager klassen toofix Fragmentera kartan problem](sql-database-elastic-database-recovery-manager.md).

![Elastisk skala villkor][1]

**Databasen**: en Azure SQL-databas. 

**Data beroende routning**: hello funktioner som gör att ett program tooconnect tooa Fragmentera ges en specifik horisontell partitionering nyckel. Se [Data beroende routning](sql-database-elastic-scale-data-dependent-routing.md). Jämför för**[flera Fragmentera frågan](sql-database-elastic-scale-multishard-querying.md)**.

**Globala Fragmentera kartan**: hello mappning mellan horisontell partitionering nycklar och deras respektive shards inom en **Fragmentera set**. hello globala Fragmentera kartan lagras i hello **Fragmentera kartan manager**. Jämför för**lokala Fragmentera kartan**.

**Lista Fragmentera kartan**: en Fragmentera karta i vilka horisontell partitionering nycklar är mappade individuellt. Jämför för**intervallet Fragmentera kartan**.   

**Lokala Fragmentera kartan**: lagras på en Fragmentera hello lokala Fragmentera mappning som innehåller mappningar för hello shardlets som finns på hello Fragmentera.

**Flera Fragmentera frågan**: hello möjlighet tooissue en fråga mot flera delar; anger resultat returneras med hjälp av UNION ALL semantik (även kallat ”fan-out query”). Jämför för**data beroende routning**.

**Flera innehavare** och **stöd för en innehavare**: här visas en enskild klient-databas och en databas med flera innehavare:

![Enstaka och flera innehavare](./media/sql-database-elastic-scale-glossary/multi-single-simple.png)

Här är en representation av **delat** enstaka och flera innehavare. 

![Enstaka och flera innehavare](./media/sql-database-elastic-scale-glossary/shards-single-multi.png)

**Intervallet Fragmentera kartan**: en Fragmentera karta i vilka hello Fragmentera distributionsstrategi baseras på flera intervall med sammanhängande värden. 

**Refererar till tabeller**: tabeller som inte är delat men replikeras över shards. Postnummer kan till exempel lagras i en tabell. 

**Fragmentera**: en Azure SQL-databas som lagrar data i ett delat datamängd. 

**Fragmentera elasticitet**: hello möjlighet tooperform båda **teckenbredden** och **lodräta skalning**.

**Delat tabeller**: tabeller som är delat, d.v.s., distribueras vars data på shards baserat på deras nyckelvärden för horisontell partitionering. 

**Horisontell partitionering nyckeln**: ett kolumnvärde som avgör hur data distribueras över shards. hello värdetypen kan vara något av följande hello: **int**, **bigint**, **varbinary**, eller **uniqueidentifier**. 

**Fragmentera set**: hello samling delar som är attributet toohello samma Fragmentera kartan i hello Fragmentera kartan manager.  

**Shardlet**: alla hello data som är associerade med ett värde för en delning nyckel på en Fragmentera. En shardlet är hello minsta möjliga dataflyttning när omdistribuera delat tabeller. 

**Fragmentera kartan**: hello uppsättning mappningar mellan horisontell partitionering nycklar och deras respektive shards.

**Fragmentera kartan manager**: en management-objekt och i datalagret som innehåller hello Fragmentera mappningen, Fragmentera platser och mappningar för en eller flera Fragmentera uppsättningar.

![Mappningar][2]

## <a name="verbs"></a>Verb
**Teckenbredden**: hello åtgärd skala ut (eller i) en mängd shards genom att lägga till eller ta bort delar tooa Fragmentera kartan enligt nedan.

![Vågräta och lodräta skalning][3]

**Sammanfoga**: hello act flyttar shardlets från två delar tooone Fragmentera och uppdaterar hello Fragmentera kartan därefter.

**Shardlet flytta**: hello act för att flytta en enda shardlet tooa olika Fragmentera. 

**Fragmentera**: hello act vågrätt partitionering identiskt strukturerade data över flera databaser baserat på en nyckel för horisontell partitionering.

**Dela**: hello act flyttas flera shardlets från en Fragmentera tooanother (vanligtvis nya) Fragmentera. Hello användaren som en nyckel för horisontell partitionering som hello dela punkt.

**Lodrät skalning**: hello åtgärd skala upp (eller ned) hello prestandanivåerna för en enskild Fragmentera. Till exempel ändrar en Fragmentera från Standard tooPremium (vilket resulterar i mer datorresurser). 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-glossary/glossary.png
[2]: ./media/sql-database-elastic-scale-glossary/mappings.png
[3]: ./media/sql-database-elastic-scale-glossary/h_versus_vert.png

