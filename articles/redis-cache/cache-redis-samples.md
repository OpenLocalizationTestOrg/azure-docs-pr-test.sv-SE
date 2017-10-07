---
title: aaaAzure Redis-Cache prover | Microsoft Docs
description: "Lär dig hur toouse Azure Redis-Cache"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 1f8d210c-ee09-4fe2-b63f-1e69246a27d8
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2017
ms.author: sdanie
ms.openlocfilehash: 5cf9287b577758b5d880d1ca3928c1bee643a8ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-redis-cache-samples"></a>Azure Redis-Cache-exempel
Det här avsnittet innehåller en lista över Azure Redis-Cache sampel, som omfattar scenarier, till exempel ansluter tooa cache, läsning och skrivning av data tooand från cache och använda hello Redis-Cache för ASP.NET-providers. Vissa av hello prover är nedladdningsbara projekt och vissa steg för steg-vägledning och innehåller kodavsnitt men inte länkar tooa nedladdningsbara projektet.

## <a name="hello-world-samples"></a>Hello world-exempel
hello exemplen i det här avsnittet visar hello grunderna i ansluter tooan Azure Redis-Cache-instans och läsning och skrivning av data toohello cacheminne med hjälp av flera olika språk och Redis-klienter.

Hej [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel visar hur tooperform olika cachelagra åtgärder med hjälp av hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-klienten.

Det här exemplet visas hur du:

* Använd olika anslutningsalternativ
* Läsa och skriva objekt tooand från hello cacheminne med hjälp av synkrona och asynkrona åtgärder
* Använd Redis MGET/MSET kommandon tooreturn värden för angivna nycklar
* Utföra Redis transaktionella åtgärder
* Arbeta med Redis listor och sorterade uppsättningar
* Lagra .NET-objekt med hjälp av JsonConvert serializers
* Använd Redis anger tooimplement taggning
* Arbeta med Redis-kluster

Mer information finns i hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentation på github och för flera Användningsscenarier finns hello [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) kontroller.

[Hur toouse Azure Redis-Cache med Python](cache-python-get-started.md) visar hur tooget igång med Azure Redis-Cache med hjälp av Python och hello [redis-kopiera](https://github.com/andymccurdy/redis-py) klienten.

[Arbeta med .NET-objekt i hello cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) visar du enkelriktade tooserialize .NET objekt så att skriva tooand läsa dem från en Azure Redis-Cache-instans. 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a>Använda Redis-Cache som en skalbar bakplan för ASP.NET SignalR
Hej [använda Redis-Cache som en skalbar bakplan för ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) exemplet visar hur du kan använda Azure Redis-Cache som ett SignalR bakplan. Läs mer om bakplan [SignalR Scaleout med Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).

## <a name="redis-cache-customer-query-sample"></a>Redis-Cache kunden frågan exempel
Det här exemplet visar jämför prestanda mellan kommer åt data från en cache och kommer åt data från beständiga lagring. Det här exemplet har två projekt.

* [Demonstrera hur Redis-Cache kan förbättra prestanda genom att cachelagra data](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [Seed hello databasen och Cache för hello demo](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a>Sessionstillståndet ASP.NET och cachelagring av utdata
Hej [Använd Azure Redis-Cache toostore ASP.NET SessionState och OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) exemplet visar hur du toouse Azure Redis-Cache toostore ASP.NET-sessionen och utdatacache med hello SessionState och OutputCache providers för Redis.

## <a name="manage-azure-redis-cache-with-maml"></a>Hantera Azure Redis-Cache med MAML
Hej [hantera Azure Redis-Cache med hjälp av Azure Management Libraries](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exemplet visar hur du använder Azure Management Libraries toomanage - (skapa / uppdatera / ta bort) ditt cacheminne. 

## <a name="custom-monitoring-sample"></a>Anpassad övervakning exempel
Hej [åtkomst Redis-Cache övervakning data](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) exemplet visar hur du kan komma åt övervakningsdata för din Azure Redis-Cache utanför hello Azure-portalen.

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a>En Twitter-format klon som skrivits med PHP och Redis
Hej [Retwis](https://github.com/SyntaxC4-MSFT/retwis) prov är hello Redis Hello World. Det är en minimal Twitter-format sociala nätverk klon som skrivits med Redis och PHP med hello [Predis](https://github.com/nrk/predis) klienten. hello källkoden är utformad toobe väldigt enkelt på hello samma tid tooshow datastrukturer olika Redis.

## <a name="bandwidth-monitor"></a>Övervakare för bandbredd
Hej [bandbredd övervakaren](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) exempel kan du toomonitor hello bandbredden på hello-klienten. toomeasure hello bandbredd, kör hello exempel på klientdatorn för hello cache, göra anrop toohello cache och se hello bandbredd som rapporterats av hello bandbredd övervakaren exempel.

