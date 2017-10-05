---
title: Azure Redis-Cache-exempel | Microsoft Docs
description: "Lär dig hur du använder Azure Redis-Cache"
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
ms.openlocfilehash: 7841fcf0b5f4dcb409abf8bfb804c2e03dad6d3a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-redis-cache-samples"></a><span data-ttu-id="f6c54-103">Azure Redis-Cache-exempel</span><span class="sxs-lookup"><span data-stu-id="f6c54-103">Azure Redis Cache samples</span></span>
<span data-ttu-id="f6c54-104">Det här avsnittet innehåller en lista över Azure Redis-Cache sampel, som omfattar scenarier, till exempel ansluta till en cache, läsa och skriva data till och från cache och använda Redis-Cache för ASP.NET-providers.</span><span class="sxs-lookup"><span data-stu-id="f6c54-104">This topic provides a list of Azure Redis Cache samples, covering scenarios such as connecting to a cache, reading and writing data to and from a cache, and using the ASP.NET Redis Cache providers.</span></span> <span data-ttu-id="f6c54-105">Några exempel är nedladdningsbara projekt och vissa steg för steg-vägledning och innehåller kodavsnitt men Länka inte till ett hämtningsbart projekt.</span><span class="sxs-lookup"><span data-stu-id="f6c54-105">Some of the samples are downloadable projects, and some provide step-by-step guidance and include code snippets but do not link to a downloadable project.</span></span>

## <a name="hello-world-samples"></a><span data-ttu-id="f6c54-106">Hello world-exempel</span><span class="sxs-lookup"><span data-stu-id="f6c54-106">Hello world samples</span></span>
<span data-ttu-id="f6c54-107">Exemplen i det här avsnittet beskrivs grunderna för att ansluta till Azure Redis-Cache-instans och läser och skriver data till cachen med olika språk och Redis-klienter.</span><span class="sxs-lookup"><span data-stu-id="f6c54-107">The samples in this section show the basics of connecting to an Azure Redis Cache instance and reading and writing data to the cache using a variety of languages and Redis clients.</span></span>

<span data-ttu-id="f6c54-108">Den [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel visas hur du utför olika cache-åtgärder med hjälp av den [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET-klienten.</span><span class="sxs-lookup"><span data-stu-id="f6c54-108">The [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample shows how to perform various cache operations using the [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) .NET client.</span></span>

<span data-ttu-id="f6c54-109">Det här exemplet visas hur du:</span><span class="sxs-lookup"><span data-stu-id="f6c54-109">This sample shows how to:</span></span>

* <span data-ttu-id="f6c54-110">Använd olika anslutningsalternativ</span><span class="sxs-lookup"><span data-stu-id="f6c54-110">Use various connection options</span></span>
* <span data-ttu-id="f6c54-111">Läsa och skriva objekt till och från cachen synkrona och asynkrona åtgärder</span><span class="sxs-lookup"><span data-stu-id="f6c54-111">Read and write objects to and from the cache using synchronous and asynchronous operations</span></span>
* <span data-ttu-id="f6c54-112">Använda Redis MGET/MSET kommandon för att returnera värden av angivna nycklar</span><span class="sxs-lookup"><span data-stu-id="f6c54-112">Use Redis MGET/MSET commands to return values of specified keys</span></span>
* <span data-ttu-id="f6c54-113">Utföra Redis transaktionella åtgärder</span><span class="sxs-lookup"><span data-stu-id="f6c54-113">Perform Redis transactional operations</span></span>
* <span data-ttu-id="f6c54-114">Arbeta med Redis listor och sorterade uppsättningar</span><span class="sxs-lookup"><span data-stu-id="f6c54-114">Work with Redis lists and sorted sets</span></span>
* <span data-ttu-id="f6c54-115">Lagra .NET-objekt med hjälp av JsonConvert serializers</span><span class="sxs-lookup"><span data-stu-id="f6c54-115">Store .NET objects using JsonConvert serializers</span></span>
* <span data-ttu-id="f6c54-116">Använd Redis anger för att implementera taggning</span><span class="sxs-lookup"><span data-stu-id="f6c54-116">Use Redis sets to implement tagging</span></span>
* <span data-ttu-id="f6c54-117">Arbeta med Redis-kluster</span><span class="sxs-lookup"><span data-stu-id="f6c54-117">Work with Redis Cluster</span></span>

<span data-ttu-id="f6c54-118">Mer information finns i [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) dokumentation på github och för flera Användningsscenarier finns i [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) kontroller.</span><span class="sxs-lookup"><span data-stu-id="f6c54-118">For more information, see the [StackExchange.Redis](https://github.com/StackExchange/StackExchange.Redis) documentation on github, and for more usage scenarios see the [StackExchange.Redis.Tests](https://github.com/StackExchange/StackExchange.Redis/tree/master/StackExchange.Redis.Tests) unit tests.</span></span>

<span data-ttu-id="f6c54-119">[Hur du använder Azure Redis-Cache med Python](cache-python-get-started.md) visar hur du kommer igång med Azure Redis-Cache med hjälp av Python och [redis-kopiera](https://github.com/andymccurdy/redis-py) klienten.</span><span class="sxs-lookup"><span data-stu-id="f6c54-119">[How to use Azure Redis Cache with Python](cache-python-get-started.md) shows how to get started with Azure Redis Cache using Python and the [redis-py](https://github.com/andymccurdy/redis-py) client.</span></span>

<span data-ttu-id="f6c54-120">[Arbeta med .NET-objekt i cacheminnet](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) visar ett sätt att serialisera .NET objekt så att du kan skriva dem till och läsa dem från en Azure Redis-Cache-instans.</span><span class="sxs-lookup"><span data-stu-id="f6c54-120">[Work with .NET objects in the cache](cache-dotnet-how-to-use-azure-redis-cache.md#work-with-net-objects-in-the-cache) shows you one way to serialize .NET objects so you can write them to and read them from an Azure Redis Cache instance.</span></span> 

## <a name="use-redis-cache-as-a-scale-out-backplane-for-aspnet-signalr"></a><span data-ttu-id="f6c54-121">Använda Redis-Cache som en skalbar bakplan för ASP.NET SignalR</span><span class="sxs-lookup"><span data-stu-id="f6c54-121">Use Redis Cache as a Scale out Backplane for ASP.NET SignalR</span></span>
<span data-ttu-id="f6c54-122">Den [använda Redis-Cache som en skalbar bakplan för ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) exemplet visar hur du kan använda Azure Redis-Cache som ett SignalR bakplan.</span><span class="sxs-lookup"><span data-stu-id="f6c54-122">The [Use Redis Cache as a Scale out Backplane for ASP.NET SignalR](https://github.com/rustd/RedisSamples/tree/master/RedisAsSignalRBackplane) sample demonstrates how you can use Azure Redis Cache as a SignalR backplane.</span></span> <span data-ttu-id="f6c54-123">Läs mer om bakplan [SignalR Scaleout med Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).</span><span class="sxs-lookup"><span data-stu-id="f6c54-123">For more information about backplane, see [SignalR Scaleout with Redis](http://www.asp.net/signalr/overview/performance/scaleout-with-redis).</span></span>

## <a name="redis-cache-customer-query-sample"></a><span data-ttu-id="f6c54-124">Redis-Cache kunden frågan exempel</span><span class="sxs-lookup"><span data-stu-id="f6c54-124">Redis Cache customer query sample</span></span>
<span data-ttu-id="f6c54-125">Det här exemplet visar jämför prestanda mellan kommer åt data från en cache och kommer åt data från beständiga lagring.</span><span class="sxs-lookup"><span data-stu-id="f6c54-125">This sample demonstrates compares performance between accessing data from a cache and accessing data from persistence storage.</span></span> <span data-ttu-id="f6c54-126">Det här exemplet har två projekt.</span><span class="sxs-lookup"><span data-stu-id="f6c54-126">This sample has two projects.</span></span>

* [<span data-ttu-id="f6c54-127">Demonstrera hur Redis-Cache kan förbättra prestanda genom att cachelagra data</span><span class="sxs-lookup"><span data-stu-id="f6c54-127">Demo how Redis Cache can improve performance by Caching data</span></span>](https://github.com/rustd/RedisSamples/tree/master/RedisCacheCustomerQuerySample)
* [<span data-ttu-id="f6c54-128">Startvärde för databasen och Cache för demonstrationen</span><span class="sxs-lookup"><span data-stu-id="f6c54-128">Seed the Database and Cache for the demo</span></span>](https://github.com/rustd/RedisSamples/tree/master/SeedCacheForCustomerQuerySample)

## <a name="aspnet-session-state-and-output-caching"></a><span data-ttu-id="f6c54-129">Sessionstillståndet ASP.NET och cachelagring av utdata</span><span class="sxs-lookup"><span data-stu-id="f6c54-129">ASP.NET Session State and Output Caching</span></span>
<span data-ttu-id="f6c54-130">Den [använder Azure Redis-Cache för lagring av ASP.NET SessionState och OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) exemplet visar hur du använder Azure Redis-Cache för att lagra ASP.NET-sessionen och utdatacache med SessionState och OutputCache providers för Redis.</span><span class="sxs-lookup"><span data-stu-id="f6c54-130">The [Use Azure Redis Cache to store ASP.NET SessionState and OutputCache](https://github.com/rustd/RedisSamples/tree/master/SessionState_OutputCaching) sample demonstrates how you to use Azure Redis Cache to store ASP.NET Session and Output Cache using the SessionState and OutputCache providers for Redis.</span></span>

## <a name="manage-azure-redis-cache-with-maml"></a><span data-ttu-id="f6c54-131">Hantera Azure Redis-Cache med MAML</span><span class="sxs-lookup"><span data-stu-id="f6c54-131">Manage Azure Redis Cache with MAML</span></span>
<span data-ttu-id="f6c54-132">Den [hantera Azure Redis-Cache med hjälp av Azure Management Libraries](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exemplet visar hur kan du använda Azure bibliotek hantering – (skapa / uppdatera / ta bort) ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="f6c54-132">The [Manage Azure Redis Cache using Azure Management Libraries](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample demonstrates how can you use Azure Management Libraries to manage - (Create/ Update/ delete) your Cache.</span></span> 

## <a name="custom-monitoring-sample"></a><span data-ttu-id="f6c54-133">Anpassad övervakning exempel</span><span class="sxs-lookup"><span data-stu-id="f6c54-133">Custom monitoring sample</span></span>
<span data-ttu-id="f6c54-134">Den [åtkomst Redis-Cache övervakning data](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) exemplet visar hur du kan komma åt övervakningsdata för din Azure Redis-Cache utanför Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="f6c54-134">The [Access Redis Cache Monitoring data](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) sample demonstrates how you can access monitoring data for your Azure Redis Cache outside of the Azure Portal.</span></span>

## <a name="a-twitter-style-clone-written-using-php-and-redis"></a><span data-ttu-id="f6c54-135">En Twitter-format klon som skrivits med PHP och Redis</span><span class="sxs-lookup"><span data-stu-id="f6c54-135">A Twitter-style clone written using PHP and Redis</span></span>
<span data-ttu-id="f6c54-136">Den [Retwis](https://github.com/SyntaxC4-MSFT/retwis) exempel är den Redis Hello World.</span><span class="sxs-lookup"><span data-stu-id="f6c54-136">The [Retwis](https://github.com/SyntaxC4-MSFT/retwis) sample is the Redis Hello World.</span></span> <span data-ttu-id="f6c54-137">Det är en minimal Twitter-format sociala nätverk klon som skrivits med Redis och PHP använder den [Predis](https://github.com/nrk/predis) klienten.</span><span class="sxs-lookup"><span data-stu-id="f6c54-137">It is a minimal Twitter-style social network clone written using Redis and PHP using the [Predis](https://github.com/nrk/predis) client.</span></span> <span data-ttu-id="f6c54-138">Källkoden är avsedd att vara mycket enkla och samtidigt för att visa olika Redis datastrukturer.</span><span class="sxs-lookup"><span data-stu-id="f6c54-138">The source code is designed to be very simple and at the same time to show different Redis data structures.</span></span>

## <a name="bandwidth-monitor"></a><span data-ttu-id="f6c54-139">Övervakare för bandbredd</span><span class="sxs-lookup"><span data-stu-id="f6c54-139">Bandwidth monitor</span></span>
<span data-ttu-id="f6c54-140">Den [bandbredd övervakaren](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) exempel kan du övervaka den bandbredd som används på klienten.</span><span class="sxs-lookup"><span data-stu-id="f6c54-140">The [Bandwidth monitor](https://github.com/JonCole/SampleCode/tree/master/BandWidthMonitor) sample allows you to monitor the bandwidth used on the client.</span></span> <span data-ttu-id="f6c54-141">För att mäta bandbredd, köra exemplet på klientdatorn cache, göra anrop till cacheminnet och se den bandbredd som rapporterats av bandbredd övervakaren exemplet.</span><span class="sxs-lookup"><span data-stu-id="f6c54-141">To measure the bandwidth, run the sample on the cache client machine, make calls to the cache, and observe the bandwidth reported by the bandwidth monitor sample.</span></span>

