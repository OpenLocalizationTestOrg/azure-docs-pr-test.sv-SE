---
title: "aaaHow tooconfigure Redis klustring för Premium Azure Redis-Cache | Microsoft Docs"
description: "Lär dig hur toocreate och hantera Redis klustring för Premium-nivån Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 62208eec-52ae-4713-b077-62659fd844ab
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: 44d520facb9d1af145b69f1b58f082aabb655d4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-redis-clustering-for-a-premium-azure-redis-cache"></a>Hur tooconfigure Redis klustring för Premium Azure Redis-Cache
Azure Redis-Cache har olika cache erbjudanden, vilket ger flexibilitet i hello valet av cachestorlek och funktioner, inklusive funktioner för Premium-nivån, till exempel klustring, beständiga och stöd för virtuella nätverk. Den här artikeln beskriver hur tooconfigure kluster i en premium Azure Redis-Cache instans.

Information om andra cache premium-funktioner finns [introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md).

## <a name="what-is-redis-cluster"></a>Vad är Redis-kluster?
Azure Redis-Cache erbjuder Redis cluster som [implementeras i Redis](http://redis.io/topics/cluster-tutorial). Med Redis-kluster får du hello följande fördelar: 

* hello möjlighet tooautomatically dela din datauppsättning mellan flera noder. 
* hello möjlighet toocontinue åtgärder när en delmängd av hello noder har uppstått fel eller är toocommunicate med hello resten av hello klustret. 
* Flera genomströmning: genomströmning linjärt ökar du ökar hello antalet delar. 
* Flera minnesstorleken: linjärt ökar du ökar hello antalet delar.  

Mer information om storlek, genomflöde och bandbredd med premium cacheminnen finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

I Azure erbjuds Redis cluster som primär replik/modell där varje Fragmentera har två primära/replik med replikering där hello replikering hanteras av Azure Redis-Cache-tjänsten. 

## <a name="clustering"></a>Klustring
Kluster har aktiverats på hello **nytt Redis-Cache** bladet under skapande av cachen. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

Kluster har konfigurerats på hello **Redis Cluster** bladet.

![Klustring][redis-cache-clustering]

Du kan ha upp too10 delar i hello kluster. Klicka på **aktiverad** och dra reglaget hello eller ange ett tal mellan 1 och 10 för **Fragmentera antal** och på **OK**.

Varje Fragmentera är en primär/replik cache-par som hanteras av Azure och hello hello-cachens totala storlek beräknas genom att multiplicera hello antalet delar med hello cachestorleken som valts i hello prisnivån. 

![Klustring][redis-cache-clustering-selected]

När hello-cache skapas du ansluta tooit och Använd den precis som en icke-klustrade cache och Redis distribuerar hello data i hela hello Cache shards. Om diagnostik är [aktiverat](cache-how-to-monitor.md#enable-cache-diagnostics), mätvärden fångas separat för varje Fragmentera och kan vara [visas](cache-how-to-monitor.md) hello Redis-Cache-bladet. 

> [!NOTE]
> 
> Det finns några mindre skillnader som krävs i ditt klientprogram när kluster är konfigurerat. Mer information finns i [behöver jag toomake eventuella ändringar toomy klienten programmet toouse kluster?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
> 
> 

Exempel om hur du arbetar med kluster med hello StackExchange.Redis klient, finns hello [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) del av hello [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel.

<a name="cluster-size"></a>

## <a name="change-hello-cluster-size-on-a-running-premium-cache"></a>Ändra hello klusterstorleken på ett premium-cache
toochange hello klusterstorleken på körs premium cachelagra med klustring aktiverad, klicka på **Redis klusterstorleken** från hello **resurs menyn**.

> [!NOTE]
> Medan hello Azure Redis Cache Premium nivå har publicerat tooGeneral tillgänglighet, är hello Redis klusterstorleken funktionen för närvarande i förhandsvisning.
> 
> 

![Redis klusterstorleken][redis-cache-redis-cluster-size]

klusterstorleken för toochange hello hello skjutreglaget eller ange ett tal mellan 1 och 10 i hello **Fragmentera antal** textrutan och klicka på **OK** toosave.

> [!NOTE]
> Skala ett kluster kör hello [MIGRERA](https://redis.io/commands/migrate) kommandot, vilket är en kostsam kommando så för minimal påverkan Överväg att köra den här åtgärden vid låg belastning. Under hello migreringsprocessen visas en topp i belastningen på servern. Skala ett kluster körs lång processen och hello mängden tid det tar beror på hello antal nycklar och storleken på hello värden som är associerade med nycklarna.
> 
> 

## <a name="clustering-faq"></a>Kluster för vanliga frågor och svar
hello innehåller följande lista svar toocommonly frågor och svar om Azure Redis-Cache-kluster.

* [Behöver jag toomake eventuella ändringar toomy klienten programmet toouse kluster?](#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)
* [Hur distribueras nycklar i ett kluster?](#how-are-keys-distributed-in-a-cluster)
* [Vad är hello största cachestorleken kan jag skapa?](#what-is-the-largest-cache-size-i-can-create)
* [Stöder alla Redis-klienter kluster?](#do-all-redis-clients-support-clustering)
* [Hur ansluter toomy cache när kluster aktiveras?](#how-do-i-connect-to-my-cache-when-clustering-is-enabled)
* [Kan jag ansluta direkt toohello enskilda delar min-cache?](#can-i-directly-connect-to-the-individual-shards-of-my-cache)
* [Kan jag konfigurera klustring för en tidigare skapad cache?](#can-i-configure-clustering-for-a-previously-created-cache)
* [Kan jag konfigurera klustring för basic eller standard-cache?](#can-i-configure-clustering-for-a-basic-or-standard-cache)
* [Kan jag använda kluster med hello Redis ASP.NET Session State och cachelagring av utdata providers?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)
* [Jag får flytta undantag när du använder StackExchange.Redis och klustring, vad ska jag göra?](#i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do)

### <a name="do-i-need-toomake-any-changes-toomy-client-application-toouse-clustering"></a>Behöver jag toomake eventuella ändringar toomy klienten programmet toouse kluster?
* När kluster aktiveras är endast databasen 0 tillgänglig. Om klientprogrammet använder flera databaser och försök tooread eller skriva tooa databas än 0, undantagsfel hello följande. `Unhandled Exception: StackExchange.Redis.RedisConnectionException: ProtocolFailure on GET --->` `StackExchange.Redis.RedisCommandException: Multiple databases are not supported on this server; cannot switch toodatabase: 6`
  
  Mer information finns i [Redis-kluster specifikation - implementerad delmängd](http://redis.io/topics/cluster-spec#implemented-subset).
* Om du använder [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/), måste du använda 1.0.481 eller senare. Du ansluter toohello cache med hjälp av hello samma [slutpunkter, portarna och nycklarna](cache-configure.md#properties) som du använder när du ansluter tooa cache som inte har aktiverat-kluster. hello enda skillnaden är att alla läsningar och skrivningar måste göras toodatabase 0.
  
  * Andra klienter kan ha olika krav. Se [alla Redis-klienter stöder kluster?](#do-all-redis-clients-support-clustering)
* Om programmet använder flera viktiga åtgärder batchar till ett enda kommando, alla nycklar måste finnas i hello samma Fragmentera. toolocate nycklar i Hej samma fragment, se [hur nycklar distribueras i ett kluster?](#how-are-keys-distributed-in-a-cluster)
* Om du använder sessionstillståndet ASP.NET för Redis-providern måste du använda 2.0.1 eller högre. Se [kan du använda kluster med hello Redis ASP.NET Session State och cachelagring av utdata providers?](#can-i-use-clustering-with-the-redis-aspnet-session-state-and-output-caching-providers)

### <a name="how-are-keys-distributed-in-a-cluster"></a>Hur distribueras nycklar i ett kluster?
Per hello Redis [nycklar distributionsmodell](http://redis.io/topics/cluster-spec#keys-distribution-model) dokumentation: hello nyckel kan delas in i 16384 platser. Varje nyckel hashas och tilldelade tooone dessa fack som distribueras över hello klusternoder hello. Du kan konfigurera vilken del av hello nyckel är hashformaterats tooensure som flera nycklar finns i hello samma Fragmentera med hash-taggar.

* Nycklar med en tagg för hash - om någon del av nyckeln hello omges av `{` och `}`, endast den del av nyckeln hello hashas för hello att fastställa hello hash-plats i en nyckel. Till exempel hello följande 3 nycklarna skulle finnas i hello samma Fragmentera: `{key}1`, `{key}2`, och `{key}3` eftersom endast hello `key` hello namnet hashas. En fullständig lista över nycklar hash-tagg specifikationer finns [nycklar hash-taggar](http://redis.io/topics/cluster-spec#keys-hash-tags).
* Nycklar utan en hash-tagg - hello hela nyckelnamn används för hashing. Detta resulterar i en statistiskt jämn fördelning över hello shards hello-cache.

För bästa prestanda och genomflöde fördela hello nycklar jämnt. Om du använder nycklar med en hash-tagg är hello programmet ansvar tooensure hello nycklar jämnt.

Mer information finns i [nycklar distributionsmodell](http://redis.io/topics/cluster-spec#keys-distribution-model), [Redis Cluster data horisontell partitionering](http://redis.io/topics/cluster-tutorial#redis-cluster-data-sharding), och [nycklar hash-taggar](http://redis.io/topics/cluster-spec#keys-hash-tags).

Exempel om hur du arbetar med kluster och hitta nycklar i hello samma Fragmentera med hello StackExchange.Redis klienten finns hello [clustering.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/Clustering.cs) del av hello [Hello World](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel.

### <a name="what-is-hello-largest-cache-size-i-can-create"></a>Vad är hello största cachestorleken kan jag skapa?
hello största premium cachestorleken är 53 GB. Du kan skapa upp too10 shards ger dig en maximal storlek på 530 GB. Om du behöver en större kan du [begära mer](mailto:wapteams@microsoft.com?subject=Redis%20Cache%20quota%20increase). Mer information finns i [priser för Azure Redis-Cache](https://azure.microsoft.com/pricing/details/cache/).

### <a name="do-all-redis-clients-support-clustering"></a>Stöder alla Redis-klienter kluster?
Vid hello inte alla klienter stöder nu Redis-kluster. StackExchange.Redis är ett som har stöd för den. Finns mer information om andra klienter hello [spelar med hello klustret](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) avsnitt i hello [Redis cluster kursen](http://redis.io/topics/cluster-tutorial).

> [!NOTE]
> Om du använder StackExchange.Redis som klient, se till att du använder hello senaste versionen av [StackExchange.Redis](https://www.nuget.org/packages/StackExchange.Redis/) 1.0.481 eller senare för klustring toowork korrekt. Om du har problem med flytta undantag, se [flytta undantag](#move-exceptions) för mer information.
> 
> 

### <a name="how-do-i-connect-toomy-cache-when-clustering-is-enabled"></a>Hur ansluter toomy cache när kluster aktiveras?
Du kan ansluta tooyour cache med hjälp av hello samma [slutpunkter](cache-configure.md#properties), [portar](cache-configure.md#properties), och [nycklar](cache-configure.md#access-keys) som du använder när du ansluter tooa cache som inte har aktiverat-kluster. Redis hanterar hello kluster på hello serverdel så att du inte behöver toomanage från klienten.

### <a name="can-i-directly-connect-toohello-individual-shards-of-my-cache"></a>Kan jag ansluta direkt toohello enskilda delar min-cache?
Detta stöds inte officiellt. Med som säger består varje Fragmentera av en primär/replik cache-par gemensamt kallas en cache-instans. Du kan ansluta toothese cacheinstanser med hello redis-cli-verktyget i hello [instabilt](http://redis.io/download) grenen av hello Redis-databasen på GitHub. Den här versionen implementerar grundläggande stöd när den startas med hello `-c` växla. Mer information finns i [spelar med hello klustret](http://redis.io/topics/cluster-tutorial#playing-with-the-cluster) på [http://redis.io](http://redis.io) i hello [Redis cluster kursen](http://redis.io/topics/cluster-tutorial).

Använd hello följande kommandon för icke-ssl.

    Redis-cli.exe –h <<cachename>> -p 13000 (tooconnect tooinstance 0)
    Redis-cli.exe –h <<cachename>> -p 13001 (tooconnect tooinstance 1)
    Redis-cli.exe –h <<cachename>> -p 13002 (tooconnect tooinstance 2)
    ...
    Redis-cli.exe –h <<cachename>> -p 1300N (tooconnect tooinstance N)

Ssl, Ersätt `1300N` med `1500N`.

### <a name="can-i-configure-clustering-for-a-previously-created-cache"></a>Kan jag konfigurera klustring för en tidigare skapad cache?
Du kan för närvarande bara aktivera kluster när du skapar en cache. Du kan ändra hello klusterstorleken efter hello-cache skapas, men du kan inte lägga till kluster tooa premium-cache eller ta bort kluster från cache premium när hello-cache skapas. Premium-cache med aktiverad klustring och endast en Fragmentera skiljer sig premium cache av hello samma storlek med utan kluster.

### <a name="can-i-configure-clustering-for-a-basic-or-standard-cache"></a>Kan jag konfigurera klustring för basic eller standard-cache?
Kluster är endast tillgängligt för premium.

### <a name="can-i-use-clustering-with-hello-redis-aspnet-session-state-and-output-caching-providers"></a>Kan jag använda kluster med hello Redis ASP.NET Session State och cachelagring av utdata providers?
* **Redis utdatacache providern** -några ändringar krävs.
* **Redis-sessionstillståndsprovider** -toouse klustring, måste du använda [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 eller högre eller ett undantagsfel genereras. Det här är en ny ändring; Mer information finns i [v2.0.0 information om ändringar i bryter](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details).

<a name="move-exceptions"></a>

### <a name="i-am-getting-move-exceptions-when-using-stackexchangeredis-and-clustering-what-should-i-do"></a>Jag får flytta undantag när du använder StackExchange.Redis och klustring, vad ska jag göra?
Om du använder StackExchange.Redis och ta emot `MOVE` undantag när du använder kluster, se till att du använder [StackExchange.Redis 1.1.603](https://www.nuget.org/packages/StackExchange.Redis/) eller senare. Anvisningar om hur du konfigurerar din .NET program toouse StackExchange.Redis finns [konfigurera hello cacheklienter](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients).

## <a name="next-steps"></a>Nästa steg
Lär dig hur toouse mer premium cachelagra funktioner.

* [Introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-clustering]: ./media/cache-how-to-premium-clustering/redis-cache-clustering.png

[redis-cache-clustering-selected]: ./media/cache-how-to-premium-clustering/redis-cache-clustering-selected.png

[redis-cache-redis-cluster-size]: ./media/cache-how-to-premium-clustering/redis-cache-redis-cluster-size.png







