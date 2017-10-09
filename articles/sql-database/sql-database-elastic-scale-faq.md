---
title: "aaaAzure SQL elastisk skala vanliga frågor och svar | Microsoft Docs"
description: "Vanliga frågor om Azure SQL Database-elastisk skalbarhet."
services: sql-database
documentationcenter: 
manager: jhubbard
author: ddove
editor: 
ms.assetid: e60dde9c-bb7b-4f2f-b52c-bdb506d49fcb
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/24/2016
ms.author: ddove
ms.openlocfilehash: 8c77902e8ce9cbbc5e081cd9d2c911d4c8dc9e5f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-tools-faq"></a>Elastisk Databasverktyg vanliga frågor och svar
#### <a name="if-i-have-a-single-tenant-per-shard-and-no-sharding-key-how-do-i-populate-hello-sharding-key-for-hello-schema-info"></a>Om jag har en enskild klient per Fragmentera och ingen nyckel för horisontell partitionering, hur jag fylla hello horisontell partitionering nyckeln hello schemat information?
hello schemaobjekt information är bara använda toosplit merge-scenarier. Om ett program är natur single-klient, kräver inte hello dela Merge tool och därför det finns inget behov av toopopulate hello schemat info objekt.

#### <a name="ive-provisioned-a-database-and-i-already-have-a-shard-map-manager-how-do-i-register-this-new-database-as-a-shard"></a>Jag har etablerats en databas och jag har redan en Fragmentera kartan Manager, hur registrerar jag den nya databasen som en Fragmentera?
Se  **[lägga till en Fragmentera tooan program med hjälp av hello klientbibliotek för elastisk databas](sql-database-elastic-scale-add-a-shard.md)**. 

#### <a name="how-much-do-elastic-database-tools-cost"></a>Hur mycket kostar elastisk Databasverktyg?
Med hjälp av hello klientbibliotek för elastisk databas medför inte några kostnader. Kostnader påförs endast för hello Azure SQL-databaser som du använder för delar och hello Fragmentera kartan Manager som du etablerar för hello dela Merge tool hello web/worker-roller.

#### <a name="why-are-my-credentials-not-working-when-i-add-a-shard-from-a-different-server"></a>Varför fungerar inte mina autentiseringsuppgifter när jag lägger till en Fragmentera från en annan server?
Använd inte autentiseringsuppgifter i hello form av ”användar-ID =username@servername”, i stället helt enkelt använda ”användar-ID = username”.  Glöm inte att hello ”användarnamn” inloggningen har behörighet för hello Fragmentera.

#### <a name="do-i-need-toocreate-a-shard-map-manager-and-populate-shards-every-time-i-start-my-applications"></a>Jag behöver toocreate en Fragmentera kartan Manager och fylla i shards varje gång startas Mina program?
Nej – hello skapandet av hello Fragmentera kartan Manager (till exempel  **[ShardMapManagerFactory.CreateSqlShardMapManager](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.createsqlshardmapmanager.aspx)**) är en engångsåtgärd.  Ditt program bör använda hello anropet  **[ShardMapManagerFactory.TryGetSqlShardMapManager()](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.trygetsqlshardmapmanager.aspx)**  vid starttiden för programmet.  Det bör bara ett sådant anrop per domän.

#### <a name="i-have-questions-about-using-elastic-database-tools-how-do-i-get-them-answered"></a>Jag har frågor om hur du använder elastisk Databasverktyg, hur skaffar jag dem besvarade?
Kontakta toous på hello [Azure SQL Database-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted).

#### <a name="when-i-get-a-database-connection-using-a-sharding-key-i-can-still-query-data-for-other-sharding-keys-on-hello-same-shard--is-this-by-design"></a>När jag får en databasanslutning med en nyckel för horisontell partitionering jag kan fortfarande fråga efter data för andra horisontell partitionering nycklar på hello samma Fragmentera.  Är detta?
hello Elastic Scale API: er ger dig en anslutning toohello rätt databas för horisontell partitionering-nyckel, men ger inte viktiga filtrering för horisontell partitionering.  Lägg till **där** satser tooyour frågan toorestrict hello scope toohello tillhandahålls horisontell partitionering nyckel, om det behövs.

#### <a name="can-i-use-a-different-azure-database-edition-for-each-shard-in-my-shard-set"></a>Kan jag använda en annan utgåva för Azure-databas för varje Fragmentera i min Fragmentera uppsättningen?
Ja, en Fragmentera är en individuell databas och därmed en Fragmentera kan vara en Premium edition medan en annan vara Standard edition. Dessutom skala hello-versionen av en Fragmentera upp eller ned flera gånger under hello livstid hello Fragmentera.

#### <a name="does-hello-split-merge-tool-provision-or-delete-a-database-during-a-split-or-merge-operation"></a>Lagrar hello dela Merge tool etablera (eller ta bort) en databas under en delade eller merge-åtgärd?
Nej. För **dela** åtgärder, hello måldatabasen måste finnas med lämpligt schema för hello och registreras med hello Fragmentera kartan Manager.  För **merge** åtgärder, måste du ta bort hello Fragmentera från hello Fragmentera kartan manager och ta sedan bort hello-databasen.

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

