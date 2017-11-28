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
# <a name="azure-redis-cache-samples"></a><span data-ttu-id="2d473-103">Azure Redis-Cache-exempel</span><span class="sxs-lookup"><span data-stu-id="2d473-103">Azure Redis Cache samples</span></span>
<span data-ttu-id="2d473-104">Det här avsnittet innehåller en lista över Azure Redis-Cache sampel, som omfattar scenarier, till exempel ansluter tooa cache, läsning och skrivning av data tooand från cache och använda hello Redis-Cache för ASP.NET-providers.</span><span class="sxs-lookup"><span data-stu-id="2d473-104">This topic provides a list of Azure Redis Cache samples, covering scenarios such as connecting tooa cache, reading and writing data tooand from a cache, and using hello ASP.NET Redis Cache providers.</span></span> <span data-ttu-id="2d473-105">Vissa av hello prover är nedladdningsbara projekt och vissa steg för steg-vägledning och innehåller kodavsnitt men inte länkar tooa nedladdningsbara projektet.</span><span class="sxs-lookup"><span data-stu-id="2d473-105">Some of hello samples are downloadable projects, and some provide step-by-step guidance and include code snippets but do not link tooa downloadable project.</span></span>

## <a name="hello-world-samples"></a><span data-ttu-id="2d473-106">Hello world-exempel</span><span class="sxs-lookup"><span data-stu-id="2d473-106">Hello world samples</span></span>
<span data-ttu-id="2d473-107">hello exemplen i det här avsnittet visar hello grunderna i ansluter tooan Azure Redis-Cache-instans och läsning och skrivning av data toohello cacheminne med hjälp av flera olika språk och Redis-klienter.</span><span class="sxs-lookup"><span data-stu-id="2d473-107">hello samples in this section show hello basics of connecting tooan Azure Redis Cache instance and reading and writing data toohello cache using a variety of languages and Redis clients.</span></span>

<span data-ttu-id="2d473-108">Hej [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel visar hur tooperform olika cachelagra åtgärder med hjälp av hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-klienten.</span><span class="sxs-lookup"><span data-stu-id="2d473-108">hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample shows how tooperform various cache operations using hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET client.</span></span>

<span data-ttu-id="2d473-109">Det här exemplet visas hur du:</span><span class="sxs-lookup"><span data-stu-id="2d473-109">This sample shows how to:</span></span>

* <span data-ttu-id="2d473-110">Använd olika anslutningsalternativ</span><span class="sxs-lookup"><span data-stu-id="2d473-110">Use various connection options</span></span>
* <span data-ttu-id="2d473-111">Läsa och skriva objekt tooand från hello cacheminne med hjälp av synkrona och asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="2d473-111">Read and write objects tooand from hello cache using synchronous and asynchronous operations</span></span>
* <span data-ttu-id="2d473-112">Använd Redis MGET/MSET kommandon tooreturn värden för angivna nycklar</span><span class="sxs-lookup"><span data-stu-id="2d473-112">Use Redis MGET/MSET commands tooreturn values of specified keys</span></span>
* <span data-ttu-id="2d473-113">Utföra Redis transaktionella åtgärder</span><span class="sxs-lookup"><span data-stu-id="2d473-113">Perform Redis transactional operations</span></span>
* <span data-ttu-id="2d473-114">Arbeta med Redis listor och sorterade uppsättningar</span><span class="sxs-lookup"><span data-stu-id="2d473-114">Work with Redis lists and sorted sets</span></span>
* <span data-ttu-id="2d473-115">Lagra .NET-objekt med hjälp av JsonConvert serializers</span><span class="sxs-lookup"><span data-stu-id="2d473-115">Store .NET objects using JsonConvert serializers</span></span>
* <span data-ttu-id="2d473-116">Använd Redis anger tooimplement taggning</span><span class="sxs-lookup"><span data-stu-id="2d473-116">Use Redis sets tooimplement tagging</span></span>
* <span data-ttu-id="2d473-117">Arbeta med Redis-kluster</span><span class="sxs-lookup"><span data-stu-id="2d473-117">Work with Redis Cluster</span></span>

<span data-ttu-id="2d473-118">Mer information finns i hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentation på github och för flera Användningsscenarier finns hello [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) kontroller.</span><span class="sxs-lookup"><span data-stu-id="2d473-118">For more information, see hello [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) documentation on github, and for more usage scenarios see hello [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) unit tests.</span></span>

<span data-ttu-id="2d473-119">[Hur toouse Azure Redis-Cache med Python](cache-python-get-started.md) visar hur tooget igång med Azure Redis-Cache med hjälp av Python och hello [redis-kopiera](https://github.com/andymccurdy/redis-py) klienten.</span><span class="sxs-lookup"><span data-stu-id="2d473-119">[How toouse Azure Redis Cache with Python](cache-python-get-started.md) shows how tooget started with Azure Redis Cache using Python and hello [redis-py](https://github.com/andymccurdy/redis-py) client.</span></span>

<span data-ttu-id="2d473-120">[Arbeta med .NET-objekt i hello cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) visar du enkelriktade tooserialize .NET objekt så att skriva tooand läsa dem från en Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="2d473-120">[Work with .NET objects in hello cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) shows you one way tooserialize .NET objects so you can write them tooand read them from an Azure Redis Cache instance.</span></span> 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a><span data-ttu-id="2d473-121">Använda Redis-Cache som en skalbar bakplan för ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="2d473-121">Use Redis Cache as a Scale out Backplane for ASP.NET SignalR</span></span>
<span data-ttu-id="2d473-122">Hej [använda Redis-Cache som en skalbar bakplan för ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) exemplet visar hur du kan använda Azure Redis-Cache som ett SignalR bakplan.</span><span class="sxs-lookup"><span data-stu-id="2d473-122">hello [Use Redis Cache as a Scale out Backplane for ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) sample demonstrates how you can use Azure Redis Cache as a SignalR backplane.</span></span> <span data-ttu-id="2d473-123">Läs mer om bakplan [SignalR Scaleout med Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).</span><span class="sxs-lookup"><span data-stu-id="2d473-123">For more information about backplane, see [SignalR Scaleout with Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).</span></span>

## <a name="redis-cache-customer-query-sample"></a><span data-ttu-id="2d473-124">Redis-Cache kunden frågan exempel</span><span class="sxs-lookup"><span data-stu-id="2d473-124">Redis Cache customer query sample</span></span>
<span data-ttu-id="2d473-125">Det här exemplet visar jämför prestanda mellan kommer åt data från en cache och kommer åt data från beständiga lagring.</span><span class="sxs-lookup"><span data-stu-id="2d473-125">This sample demonstrates compares performance between accessing data from a cache and accessing data from persistence storage.</span></span> <span data-ttu-id="2d473-126">Det här exemplet har två projekt.</span><span class="sxs-lookup"><span data-stu-id="2d473-126">This sample has two projects.</span></span>

* [<span data-ttu-id="2d473-127">Demonstrera hur Redis-Cache kan förbättra prestanda genom att cachelagra data</span><span class="sxs-lookup"><span data-stu-id="2d473-127">Demo how Redis Cache can improve performance by Caching data</span></span>](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [<span data-ttu-id="2d473-128">Seed hello databasen och Cache för hello demo</span><span class="sxs-lookup"><span data-stu-id="2d473-128">Seed hello Database and Cache for hello demo</span></span>](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a><span data-ttu-id="2d473-129">Sessionstillståndet ASP.NET och cachelagring av utdata</span><span class="sxs-lookup"><span data-stu-id="2d473-129">ASP.NET Session State and Output Caching</span></span>
<span data-ttu-id="2d473-130">Hej [Använd Azure Redis-Cache toostore ASP.NET SessionState och OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) exemplet visar hur du toouse Azure Redis-Cache toostore ASP.NET-sessionen och utdatacache med hello SessionState och OutputCache providers för Redis.</span><span class="sxs-lookup"><span data-stu-id="2d473-130">hello [Use Azure Redis Cache toostore ASP.NET SessionState and OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) sample demonstrates how you toouse Azure Redis Cache toostore ASP.NET Session and Output Cache using hello SessionState and OutputCache providers for Redis.</span></span>

## <a name="manage-azure-redis-cache-with-maml"></a><span data-ttu-id="2d473-131">Hantera Azure Redis-Cache med MAML</span><span class="sxs-lookup"><span data-stu-id="2d473-131">Manage Azure Redis Cache with MAML</span></span>
<span data-ttu-id="2d473-132">Hej [hantera Azure Redis-Cache med hjälp av Azure Management Libraries](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exemplet visar hur du använder Azure Management Libraries toomanage - (skapa / uppdatera / ta bort) ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="2d473-132">hello [Manage Azure Redis Cache using Azure Management Libraries](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample demonstrates how can you use Azure Management Libraries toomanage - (Create/ Update/ delete) your Cache.</span></span> 

## <a name="custom-monitoring-sample"></a><span data-ttu-id="2d473-133">Anpassad övervakning exempel</span><span class="sxs-lookup"><span data-stu-id="2d473-133">Custom monitoring sample</span></span>
<span data-ttu-id="2d473-134">Hej [åtkomst Redis-Cache övervakning data](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) exemplet visar hur du kan komma åt övervakningsdata för din Azure Redis-Cache utanför hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="2d473-134">hello [Access Redis Cache Monitoring data](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) sample demonstrates how you can access monitoring data for your Azure Redis Cache outside of hello Azure Portal.</span></span>

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a><span data-ttu-id="2d473-135">En Twitter-format klon som skrivits med PHP och Redis</span><span class="sxs-lookup"><span data-stu-id="2d473-135">A Twitter-style clone written using PHP and Redis</span></span>
<span data-ttu-id="2d473-136">Hej [Retwis](https://github.com/SyntaxC4-MSFT/retwis) prov är hello Redis Hello World.</span><span class="sxs-lookup"><span data-stu-id="2d473-136">hello [Retwis](https://github.com/SyntaxC4-MSFT/retwis) sample is hello Redis Hello World.</span></span> <span data-ttu-id="2d473-137">Det är en minimal Twitter-format sociala nätverk klon som skrivits med Redis och PHP med hello [Predis](https://github.com/nrk/predis) klienten.</span><span class="sxs-lookup"><span data-stu-id="2d473-137">It is a minimal Twitter-style social network clone written using Redis and PHP using hello [Predis](https://github.com/nrk/predis) client.</span></span> <span data-ttu-id="2d473-138">hello källkoden är utformad toobe väldigt enkelt på hello samma tid tooshow datastrukturer olika Redis.</span><span class="sxs-lookup"><span data-stu-id="2d473-138">hello source code is designed toobe very simple and at hello same time tooshow different Redis data structures.</span></span>

## <a name="bandwidth-monitor"></a><span data-ttu-id="2d473-139">Övervakare för bandbredd</span><span class="sxs-lookup"><span data-stu-id="2d473-139">Bandwidth monitor</span></span>
<span data-ttu-id="2d473-140">Hej [bandbredd övervakaren](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) exempel kan du toomonitor hello bandbredden på hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="2d473-140">hello [Bandwidth monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) sample allows you toomonitor hello bandwidth used on hello client.</span></span> <span data-ttu-id="2d473-141">toomeasure hello bandbredd, kör hello exempel på klientdatorn för hello cache, göra anrop toohello cache och se hello bandbredd som rapporterats av hello bandbredd övervakaren exempel.</span><span class="sxs-lookup"><span data-stu-id="2d473-141">toomeasure hello bandwidth, run hello sample on hello cache client machine, make calls toohello cache, and observe hello bandwidth reported by hello bandwidth monitor sample.</span></span>

