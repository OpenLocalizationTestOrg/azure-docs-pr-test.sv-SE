---
title: aaaHow tooUse Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooimprove hello prestandan för din Azure-program med Azure Redis-Cache"
services: redis-cache,app-service
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: c502f74c-44de-4087-8303-1b1f43da12d5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 07/27/2017
ms.author: sdanie
ms.openlocfilehash: 763d70c10972eec9a1885969e8da5bf1c4084727
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache"></a><span data-ttu-id="f7423-103">Hur tooUse Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="f7423-103">How tooUse Azure Redis Cache</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f7423-104">.NET</span><span class="sxs-lookup"><span data-stu-id="f7423-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="f7423-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7423-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="f7423-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="f7423-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="f7423-107">Java</span><span class="sxs-lookup"><span data-stu-id="f7423-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="f7423-108">Python</span><span class="sxs-lookup"><span data-stu-id="f7423-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="f7423-109">Den här guiden visar hur tooget igång med **Azure Redis-Cache**.</span><span class="sxs-lookup"><span data-stu-id="f7423-109">This guide shows you how tooget started using **Azure Redis Cache**.</span></span> <span data-ttu-id="f7423-110">Microsoft Azure Redis-Cache är baserad på hello populära öppen källkod Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-110">Microsoft Azure Redis Cache is based on hello popular open source Redis Cache.</span></span> <span data-ttu-id="f7423-111">Den ger dig åtkomst till tooa säker, dedikerad Redis-cache, hanteras av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="f7423-111">It gives you access tooa secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="f7423-112">Ett cacheminne som har skapats med Azure Redis Cache är tillgängligt från alla program i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="f7423-112">A cache created using Azure Redis Cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="f7423-113">Microsoft Azure Redis-Cache är tillgängliga i hello följande nivåer:</span><span class="sxs-lookup"><span data-stu-id="f7423-113">Microsoft Azure Redis Cache is available in hello following tiers:</span></span>

* <span data-ttu-id="f7423-114">**Grundläggande** – En nod.</span><span class="sxs-lookup"><span data-stu-id="f7423-114">**Basic** – Single node.</span></span> <span data-ttu-id="f7423-115">Flera storlekar upp too53 GB.</span><span class="sxs-lookup"><span data-stu-id="f7423-115">Multiple sizes up too53 GB.</span></span>
* <span data-ttu-id="f7423-116">**Standard** – Primär/replik med två noder.</span><span class="sxs-lookup"><span data-stu-id="f7423-116">**Standard** – Two-node Primary/Replica.</span></span> <span data-ttu-id="f7423-117">Flera storlekar upp too53 GB.</span><span class="sxs-lookup"><span data-stu-id="f7423-117">Multiple sizes up too53 GB.</span></span> <span data-ttu-id="f7423-118">99,9 % SLA.</span><span class="sxs-lookup"><span data-stu-id="f7423-118">99.9% SLA.</span></span>
* <span data-ttu-id="f7423-119">**Premium** – två noder primära/replik med upp too10 delar.</span><span class="sxs-lookup"><span data-stu-id="f7423-119">**Premium** – Two-node Primary/Replica with up too10 shards.</span></span> <span data-ttu-id="f7423-120">Flera storlekar från 6 GB too530 GB.</span><span class="sxs-lookup"><span data-stu-id="f7423-120">Multiple sizes from 6 GB too530 GB.</span></span> <span data-ttu-id="f7423-121">Alla standardnivåfunktioner med mera, med stöd för [Redis-kluster](cache-how-to-premium-clustering.md), [Redis persistence](cache-how-to-premium-persistence.md) och [Azure Virtual Network](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="f7423-121">All Standard tier features and more including support for [Redis cluster](cache-how-to-premium-clustering.md), [Redis persistence](cache-how-to-premium-persistence.md), and [Azure Virtual Network](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="f7423-122">99,9 % SLA.</span><span class="sxs-lookup"><span data-stu-id="f7423-122">99.9% SLA.</span></span>

<span data-ttu-id="f7423-123">Varje nivå har olika funktioner och priser.</span><span class="sxs-lookup"><span data-stu-id="f7423-123">Each tier differs in terms of features and pricing.</span></span> <span data-ttu-id="f7423-124">Mer information om priser finns i [prisinformationen för Cache][Cache Pricing Details].</span><span class="sxs-lookup"><span data-stu-id="f7423-124">For information on pricing, see [Cache Pricing Details][Cache Pricing Details].</span></span>

<span data-ttu-id="f7423-125">Den här guiden visar hur toouse hello [StackExchange.Redis] [ StackExchange.Redis] klienten med hjälp av C\# kod.</span><span class="sxs-lookup"><span data-stu-id="f7423-125">This guide shows you how toouse hello [StackExchange.Redis][StackExchange.Redis] client using C\# code.</span></span> <span data-ttu-id="f7423-126">hello beskrivs scenarier där **skapar och konfigurerar en cache**, **konfigurera cacheklienter**, och **att lägga till och ta bort objekt från cache hello**.</span><span class="sxs-lookup"><span data-stu-id="f7423-126">hello scenarios covered include **creating and configuring a cache**, **configuring cache clients**, and **adding and removing objects from hello cache**.</span></span> <span data-ttu-id="f7423-127">Mer information om hur du använder Azure Redis Cache finns i [Nästa steg][Next Steps].</span><span class="sxs-lookup"><span data-stu-id="f7423-127">For more information on using Azure Redis Cache, see [Next Steps][Next Steps].</span></span> <span data-ttu-id="f7423-128">Stegvisa självstudier för att skapa en ASP.NET MVC-webbapp med Redis-Cache finns [hur toocreate ett webbprogram med Redis-Cache](cache-web-app-howto.md).</span><span class="sxs-lookup"><span data-stu-id="f7423-128">For a step-by-step tutorial of building an ASP.NET MVC web app with Redis Cache, see [How toocreate a Web App with Redis Cache](cache-web-app-howto.md).</span></span>

<a name="getting-started-cache-service"></a>

## <a name="get-started-with-azure-redis-cache"></a><span data-ttu-id="f7423-129">Kom igång med Azure Redis Cache</span><span class="sxs-lookup"><span data-stu-id="f7423-129">Get Started with Azure Redis Cache</span></span>
<span data-ttu-id="f7423-130">Att komma igång med Azure Redis Cache är enkelt.</span><span class="sxs-lookup"><span data-stu-id="f7423-130">Getting started with Azure Redis Cache is easy.</span></span> <span data-ttu-id="f7423-131">tooget igång kan du etablera och konfigurera en cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-131">tooget started, you provision and configure a cache.</span></span> <span data-ttu-id="f7423-132">Därefter konfigurerar du hello cache-klienter så att de kan komma åt hello cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-132">Next, you configure hello cache clients so they can access hello cache.</span></span> <span data-ttu-id="f7423-133">Du kan börja arbeta med dem. när hello cacheklienter har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="f7423-133">Once hello cache clients are configured, you can begin working with them.</span></span>

* <span data-ttu-id="f7423-134">[Skapa hello-cache][Create hello cache]</span><span class="sxs-lookup"><span data-stu-id="f7423-134">[Create hello cache][Create hello cache]</span></span>
* <span data-ttu-id="f7423-135">[Konfigurera hello cache-klienter][Configure hello cache clients]</span><span class="sxs-lookup"><span data-stu-id="f7423-135">[Configure hello cache clients][Configure hello cache clients]</span></span>

<a name="create-cache"></a>

## <a name="create-a-cache"></a><span data-ttu-id="f7423-136">Skapa en cache</span><span class="sxs-lookup"><span data-stu-id="f7423-136">Create a cache</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

### <a name="tooaccess-your-cache-after-its-created"></a><span data-ttu-id="f7423-137">tooaccess ditt cacheminne efter att det har skapats</span><span class="sxs-lookup"><span data-stu-id="f7423-137">tooaccess your cache after it's created</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="f7423-138">Mer information om hur du konfigurerar ditt cacheminne finns [hur tooconfigure Azure Redis-Cache](cache-configure.md).</span><span class="sxs-lookup"><span data-stu-id="f7423-138">For more information about configuring your cache, see [How tooconfigure Azure Redis Cache](cache-configure.md).</span></span>

<a name="NuGet"></a>

## <a name="configure-hello-cache-clients"></a><span data-ttu-id="f7423-139">Konfigurera hello cache-klienter</span><span class="sxs-lookup"><span data-stu-id="f7423-139">Configure hello cache clients</span></span>
[!INCLUDE [redis-cache-configure](../../includes/redis-cache-configure-stackexchange-redis-nuget.md)]

<span data-ttu-id="f7423-140">Du kan använda hello tekniker som beskrivs i följande avsnitt för att arbeta med ditt cacheminne hello när klientprojektet är konfigurerad för cachelagring.</span><span class="sxs-lookup"><span data-stu-id="f7423-140">Once your client project is configured for caching, you can use hello techniques described in hello following sections for working with your cache.</span></span>

<a name="working-with-caches"></a>

## <a name="working-with-caches"></a><span data-ttu-id="f7423-141">Arbeta med cacheminnen</span><span class="sxs-lookup"><span data-stu-id="f7423-141">Working with Caches</span></span>
<span data-ttu-id="f7423-142">hello steg i det här avsnittet beskriver hur tooperform vanliga uppgifter med Cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-142">hello steps in this section describe how tooperform common tasks with Cache.</span></span>

* <span data-ttu-id="f7423-143">[Ansluta toohello cache][Connect toohello cache]</span><span class="sxs-lookup"><span data-stu-id="f7423-143">[Connect toohello cache][Connect toohello cache]</span></span>
* <span data-ttu-id="f7423-144">[Lägg till och hämta objekt från hello cache][Add and retrieve objects from hello cache]</span><span class="sxs-lookup"><span data-stu-id="f7423-144">[Add and retrieve objects from hello cache][Add and retrieve objects from hello cache]</span></span>
* [<span data-ttu-id="f7423-145">Arbeta med .NET-objekt i hello cache</span><span class="sxs-lookup"><span data-stu-id="f7423-145">Work with .NET objects in hello cache</span></span>](#work-with-net-objects-in-the-cache)

<a name="connect-to-cache"></a>

## <a name="connect-toohello-cache"></a><span data-ttu-id="f7423-146">Ansluta toohello cache</span><span class="sxs-lookup"><span data-stu-id="f7423-146">Connect toohello cache</span></span>
<span data-ttu-id="f7423-147">tooprogrammatically arbete med en cache, behöver du en referens toohello cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-147">tooprogrammatically work with a cache, you need a reference toohello cache.</span></span> <span data-ttu-id="f7423-148">Lägg till hello följande toohello upp en fil som du vill toouse hello StackExchange.Redis klienten tooaccess ett Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-148">Add hello following toohello top of any file from which you want toouse hello StackExchange.Redis client tooaccess an Azure Redis Cache.</span></span>

    using StackExchange.Redis;

> [!NOTE]
> <span data-ttu-id="f7423-149">Hej StackExchange.Redis klienten kräver .NET Framework 4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="f7423-149">hello StackExchange.Redis client requires .NET Framework 4 or higher.</span></span>
> 
> 

<span data-ttu-id="f7423-150">Hej anslutning toohello Azure Redis-Cache hanteras av hello `ConnectionMultiplexer` klass.</span><span class="sxs-lookup"><span data-stu-id="f7423-150">hello connection toohello Azure Redis Cache is managed by hello `ConnectionMultiplexer` class.</span></span> <span data-ttu-id="f7423-151">Den här klassen bör delas återanvändas i hela ditt klientprogram och behöver inte toobe som skapats på grundval av per igen.</span><span class="sxs-lookup"><span data-stu-id="f7423-151">This class should be shared and reused throughout your client application, and does not need toobe created on a per operation basis.</span></span> 

<span data-ttu-id="f7423-152">tooconnect tooan Azure Redis-Cache och returneras en instans av en ansluten `ConnectionMultiplexer`, anrop hello statiska `Connect` metoden och skicka hello cachelagra slutpunkt och nyckel.</span><span class="sxs-lookup"><span data-stu-id="f7423-152">tooconnect tooan Azure Redis Cache and be returned an instance of a connected `ConnectionMultiplexer`, call hello static `Connect` method and pass in hello cache endpoint and key.</span></span> <span data-ttu-id="f7423-153">Använd hello nyckeln genereras från hello Azure-portalen som hello lösenord parameter.</span><span class="sxs-lookup"><span data-stu-id="f7423-153">Use hello key generated from hello Azure portal as hello password parameter.</span></span>

    ConnectionMultiplexer connection = ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");

> [!IMPORTANT]
> <span data-ttu-id="f7423-154">Varning: Lagra aldrig autentiseringsuppgifterna i källkoden.</span><span class="sxs-lookup"><span data-stu-id="f7423-154">Warning: Never store credentials in source code.</span></span> <span data-ttu-id="f7423-155">tookeep det här exemplet enkel jag visar dem i hello källkoden.</span><span class="sxs-lookup"><span data-stu-id="f7423-155">tookeep this sample simple, I’m showing them in hello source code.</span></span> <span data-ttu-id="f7423-156">Se [hur programmet strängar och anslutningen strängar arbete] [ How Application Strings and Connection Strings Work] information om hur toostore autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f7423-156">See [How Application Strings and Connection Strings Work][How Application Strings and Connection Strings Work] for information on how toostore credentials.</span></span>
> 
> 

<span data-ttu-id="f7423-157">Om du inte vill toouse SSL kan antingen ange `ssl=false` eller utelämna hello `ssl` parameter.</span><span class="sxs-lookup"><span data-stu-id="f7423-157">If you don't want toouse SSL, either set `ssl=false` or omit hello `ssl` parameter.</span></span>

> [!NOTE]
> <span data-ttu-id="f7423-158">hello icke-SSL-porten är inaktiverad som standard för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="f7423-158">hello non-SSL port is disabled by default for new caches.</span></span> <span data-ttu-id="f7423-159">Anvisningar om hur du aktiverar hello icke-SSL-porten finns [Åtkomstportar](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="f7423-159">For instructions on enabling hello non-SSL port, see [Access Ports](cache-configure.md#access-ports).</span></span>
> 
> 

<span data-ttu-id="f7423-160">En metod som toosharing en `ConnectionMultiplexer` instans i ditt program är toohave en statisk egenskap som returnerar en ansluten instans, liknande toohello följande exempel.</span><span class="sxs-lookup"><span data-stu-id="f7423-160">One approach toosharing a `ConnectionMultiplexer` instance in your application is toohave a static property that returns a connected instance, similar toohello following example.</span></span> <span data-ttu-id="f7423-161">Den här metoden ger ett trådsäkert sätt tooinitialize ett enda anslutet `ConnectionMultiplexer` instans.</span><span class="sxs-lookup"><span data-stu-id="f7423-161">This approach provides a thread-safe way tooinitialize only a single connected `ConnectionMultiplexer` instance.</span></span> <span data-ttu-id="f7423-162">I de här exemplen `abortConnect` är uppsättningen toofalse, vilket innebär att hello anropet lyckas även om det inte är att upprätta en anslutning toohello Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-162">In these examples `abortConnect` is set toofalse, which means that hello call succeeds even if a connection toohello Azure Redis Cache is not established.</span></span> <span data-ttu-id="f7423-163">En nyckelfunktion i `ConnectionMultiplexer` är att automatiskt återställs anslutningen toohello cache när hello nätverksfel eller andra orsaker är löst.</span><span class="sxs-lookup"><span data-stu-id="f7423-163">One key feature of `ConnectionMultiplexer` is that it automatically restores connectivity toohello cache once hello network issue or other causes are resolved.</span></span>

    private static Lazy<ConnectionMultiplexer> lazyConnection = new Lazy<ConnectionMultiplexer>(() =>
    {
        return ConnectionMultiplexer.Connect("contoso5.redis.cache.windows.net,abortConnect=false,ssl=true,password=...");
    });

    public static ConnectionMultiplexer Connection
    {
        get
        {
            return lazyConnection.Value;
        }
    }

<span data-ttu-id="f7423-164">Mer information om avancerade alternativ för anslutningskonfiguration finns i [Konfigurationsmodell för StackExchange.Redis][StackExchange.Redis configuration model].</span><span class="sxs-lookup"><span data-stu-id="f7423-164">For more information on advanced connection configuration options, see [StackExchange.Redis configuration model][StackExchange.Redis configuration model].</span></span>

[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

<span data-ttu-id="f7423-165">När hello anslutning har upprättats kan returnera en referens toohello redis cache-databas genom att anropa hello `ConnectionMultiplexer.GetDatabase` metod.</span><span class="sxs-lookup"><span data-stu-id="f7423-165">Once hello connection is established, return a reference toohello redis cache database by calling hello `ConnectionMultiplexer.GetDatabase` method.</span></span> <span data-ttu-id="f7423-166">hello-objektet som returnerades från hello `GetDatabase` metoden är ett enkelt direkt objekt och behöver inte toobe lagras.</span><span class="sxs-lookup"><span data-stu-id="f7423-166">hello object returned from hello `GetDatabase` method is a lightweight pass-through object and does not need toobe stored.</span></span>

    // Connection refers tooa property that returns a ConnectionMultiplexer
    // as shown in hello previous example.
    IDatabase cache = Connection.GetDatabase();

    // Perform cache operations using hello cache object...
    // Simple put of integral data types into hello cache
    cache.StringSet("key1", "value");
    cache.StringSet("key2", 25);

    // Simple get of data types from hello cache
    string key1 = cache.StringGet("key1");
    int key2 = (int)cache.StringGet("key2");

<span data-ttu-id="f7423-167">Azure Redis-cache har en konfigurerbar antalet databaser (standard 16) som kan använda toologically separat hello data i ett Redis-cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-167">Azure Redis caches have a configurable number of databases (default of 16) that can be used toologically separate hello data within a Redis cache.</span></span> <span data-ttu-id="f7423-168">Mer information finns i [What are Redis databases?](cache-faq.md#what-are-redis-databases) (Vad är Redis-databaser?) och [Default Redis server configuration](cache-configure.md#default-redis-server-configuration) (Standardkonfiguration av Redis-server).</span><span class="sxs-lookup"><span data-stu-id="f7423-168">For more information, see [What are Redis databases?](cache-faq.md#what-are-redis-databases) and [Default Redis server configuration](cache-configure.md#default-redis-server-configuration).</span></span>

<span data-ttu-id="f7423-169">Nu när du vet hur tooconnect tooan Azure Redis-Cache-instansen och returnerar en referens toohello cachelagra databasen ska vi titta på arbeta med hello cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-169">Now that you know how tooconnect tooan Azure Redis Cache instance and return a reference toohello cache database, let's look at working with hello cache.</span></span>

<a name="add-object"></a>

## <a name="add-and-retrieve-objects-from-hello-cache"></a><span data-ttu-id="f7423-170">Lägg till och hämta objekt från hello cache</span><span class="sxs-lookup"><span data-stu-id="f7423-170">Add and retrieve objects from hello cache</span></span>
<span data-ttu-id="f7423-171">Objekt som kan lagras i och hämtas från en cache med hjälp av hello `StringSet` och `StringGet` metoder.</span><span class="sxs-lookup"><span data-stu-id="f7423-171">Items can be stored in and retrieved from a cache by using hello `StringSet` and `StringGet` methods.</span></span>

    // If key1 exists, it is overwritten.
    cache.StringSet("key1", "value1");

    string value = cache.StringGet("key1");

<span data-ttu-id="f7423-172">Redis lagrar de flesta data som Redis strängar, men dessa strängar kan innehålla flera typer av data, inklusive serialiserade binära data som kan användas när lagra .NET objekt i hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="f7423-172">Redis stores most data as Redis strings, but these strings can contain many types of data, including serialized binary data, which can be used when storing .NET objects in hello cache.</span></span>

<span data-ttu-id="f7423-173">När du anropar `StringGet`, om det inte finns hello objekt returneras, och om inte `null` returneras.</span><span class="sxs-lookup"><span data-stu-id="f7423-173">When calling `StringGet`, if hello object exists, it is returned, and if it does not, `null` is returned.</span></span> <span data-ttu-id="f7423-174">Om `null` returneras kan du hämta hello värdet från hello önskad datakälla och lagra den på hello cache för senare användning.</span><span class="sxs-lookup"><span data-stu-id="f7423-174">If `null` is returned, you can retrieve hello value from hello desired data source and store it in hello cache for subsequent use.</span></span> <span data-ttu-id="f7423-175">Den här användningsmönstret kallas hello cache-reservera mönster.</span><span class="sxs-lookup"><span data-stu-id="f7423-175">This usage pattern is known as hello cache-aside pattern.</span></span>

    string value = cache.StringGet("key1");
    if (value == null)
    {
        // hello item keyed by "key1" is not in hello cache. Obtain
        // it from hello desired data source and add it toohello cache.
        value = GetValueFromDataSource();

        cache.StringSet("key1", value);
    }

<span data-ttu-id="f7423-176">Du kan också använda `RedisValue`som visas i följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="f7423-176">You can also use `RedisValue`, as shown in hello following example.</span></span> <span data-ttu-id="f7423-177">`RedisValue` har implicita operatorer för att hantera integrala datatyper och kan vara användbart om `null` är ett förväntat värde för ett cachelagrat objekt.</span><span class="sxs-lookup"><span data-stu-id="f7423-177">`RedisValue` has implicit operators for working with integral data types, and can be useful if `null` is an expected value for a cached item.</span></span>


    RedisValue value = cache.StringGet("key1");
    if (!value.HasValue)
    {
        value = GetValueFromDataSource();
        cache.StringSet("key1", value);
    }


<span data-ttu-id="f7423-178">toospecify hello giltighetstid för ett objekt i hello cache, Använd hello `TimeSpan` -parametern för `StringSet`.</span><span class="sxs-lookup"><span data-stu-id="f7423-178">toospecify hello expiration of an item in hello cache, use hello `TimeSpan` parameter of `StringSet`.</span></span>

    cache.StringSet("key1", "value1", TimeSpan.FromMinutes(90));

## <a name="work-with-net-objects-in-hello-cache"></a><span data-ttu-id="f7423-179">Arbeta med .NET-objekt i hello cache</span><span class="sxs-lookup"><span data-stu-id="f7423-179">Work with .NET objects in hello cache</span></span>
<span data-ttu-id="f7423-180">Azure Redis Cache kan cachelagra både .NET-objekt och basdatatyper, men .NET-objekt måste serialiseras innan de kan cachelagras.</span><span class="sxs-lookup"><span data-stu-id="f7423-180">Azure Redis Cache can cache both .NET objects and primitive data types, but before a .NET object can be cached it must be serialized.</span></span> <span data-ttu-id="f7423-181">Det här objektet .NET-serialisering är hello programutvecklaren hello ansvar och ger hello developer flexibilitet i hello valet av hello serialiseraren.</span><span class="sxs-lookup"><span data-stu-id="f7423-181">This .NET object serialization is hello responsibility of hello application developer, and gives hello developer flexibility in hello choice of hello serializer.</span></span>

<span data-ttu-id="f7423-182">Ett enkelt sätt tooserialize objekt är toouse hello `JsonConvert` serialisering metoder i [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) och serialisera tooand från JSON.</span><span class="sxs-lookup"><span data-stu-id="f7423-182">One simple way tooserialize objects is toouse hello `JsonConvert` serialization methods in [Newtonsoft.Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/8.0.1-beta1) and serialize tooand from JSON.</span></span> <span data-ttu-id="f7423-183">hello följande exempel visas en get och set med hjälp av en `Employee` objektinstansen.</span><span class="sxs-lookup"><span data-stu-id="f7423-183">hello following example shows a get and set using an `Employee` object instance.</span></span>

    class Employee
    {
        public int Id { get; set; }
        public string Name { get; set; }

        public Employee(int EmployeeId, string Name)
        {
            this.Id = EmployeeId;
            this.Name = Name;
        }
    }

    // Store toocache
    cache.StringSet("e25", JsonConvert.SerializeObject(new Employee(25, "Clayton Gragg")));

    // Retrieve from cache
    Employee e25 = JsonConvert.DeserializeObject<Employee>(cache.StringGet("e25"));

<a name="next-steps"></a>

## <a name="next-steps"></a><span data-ttu-id="f7423-184">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f7423-184">Next Steps</span></span>
<span data-ttu-id="f7423-185">Nu när du har lärt dig grunderna i hello följa dessa länkar toolearn mer om Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-185">Now that you've learned hello basics, follow these links toolearn more about Azure Redis Cache.</span></span>

* <span data-ttu-id="f7423-186">Checka ut hello ASP.NET-providers för Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="f7423-186">Check out hello ASP.NET providers for Azure Redis Cache.</span></span>
  * [<span data-ttu-id="f7423-187">Sessionstillståndsprovider för Azure Redis</span><span class="sxs-lookup"><span data-stu-id="f7423-187">Azure Redis Session State Provider</span></span>](cache-aspnet-session-state-provider.md)
  * [<span data-ttu-id="f7423-188">Utdatacacheprovider för Azure Redis Cache ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f7423-188">Azure Redis Cache ASP.NET Output Cache Provider</span></span>](cache-aspnet-output-cache-provider.md)
* <span data-ttu-id="f7423-189">[Aktivera cache diagnostik](cache-how-to-monitor.md#enable-cache-diagnostics) så att du kan [övervakaren](cache-how-to-monitor.md) hello hälsotillståndet för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="f7423-189">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) hello health of your cache.</span></span> <span data-ttu-id="f7423-190">Du kan visa hello mått i hello Azure-portalen och du kan också [ladda ned och granska](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) dem med hello-verktyg.</span><span class="sxs-lookup"><span data-stu-id="f7423-190">You can view hello metrics in hello Azure portal and you can also [download and review](https://github.com/rustd/RedisSamples/tree/master/CustomMonitoring) them using hello tools of your choice.</span></span>
* <span data-ttu-id="f7423-191">Kolla in hello [StackExchange.Redis cache klienten dokumentationen][StackExchange.Redis cache client documentation].</span><span class="sxs-lookup"><span data-stu-id="f7423-191">Check out hello [StackExchange.Redis cache client documentation][StackExchange.Redis cache client documentation].</span></span>
  * <span data-ttu-id="f7423-192">Azure Redis Cache kan nås från många Redis-klienter och programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="f7423-192">Azure Redis Cache can be accessed from many Redis clients and development languages.</span></span> <span data-ttu-id="f7423-193">Mer information finns på [http://redis.io/clients][http://redis.io/clients].</span><span class="sxs-lookup"><span data-stu-id="f7423-193">For more information, see [http://redis.io/clients][http://redis.io/clients].</span></span>
* <span data-ttu-id="f7423-194">Azure Redis Cache kan också användas med tjänster och verktyg från tredje part som Redsmin och Redis Desktop Manager.</span><span class="sxs-lookup"><span data-stu-id="f7423-194">Azure Redis Cache can also be used with third-party services and tools such as Redsmin and Redis Desktop Manager.</span></span>
  * <span data-ttu-id="f7423-195">Läs mer om Redsmin [hur tooretrieve en anslutning för Azure Redis-sträng och använda den med Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].</span><span class="sxs-lookup"><span data-stu-id="f7423-195">For more information about Redsmin, see [How tooretrieve an Azure Redis connection string and use it with Redsmin][How tooretrieve an Azure Redis connection string and use it with Redsmin].</span></span>
  * <span data-ttu-id="f7423-196">Komma åt och granska dina data i Azure Redis Cache med ett grafiskt användargränssnitt med hjälp av [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).</span><span class="sxs-lookup"><span data-stu-id="f7423-196">Access and inspect your data in Azure Redis Cache with a GUI using [RedisDesktopManager](https://github.com/uglide/RedisDesktopManager).</span></span>
* <span data-ttu-id="f7423-197">Se hello [redis] [ redis] dokumentation och Läs om [redis datatyper] [ redis data types] och [en introduktion 15 minut tooRedis datatyper][a fifteen minute introduction tooRedis data types].</span><span class="sxs-lookup"><span data-stu-id="f7423-197">See hello [redis][redis] documentation and read about [redis data types][redis data types] and [a fifteen minute introduction tooRedis data types][a fifteen minute introduction tooRedis data types].</span></span>

<!-- INTRA-TOPIC LINKS -->
[Next Steps]: #next-steps
[Introduction tooAzure Redis Cache (Video)]: #video
[What is Azure Redis Cache?]: #what-is
[Create an Azure Cache]: #create-cache
[Which type of caching is right for me?]: #choosing-cache
[Prepare Your Visual Studio Project tooUse Azure Caching]: #prepare-vs
[Configure Your Application tooUse Caching]: #configure-app
[Get Started with Azure Redis Cache]: #getting-started-cache-service
[Create hello cache]: #create-cache
[Configure hello cache]: #enable-caching
[Configure hello cache clients]: #NuGet
[Working with Caches]: #working-with-caches
[Connect toohello cache]: #connect-to-cache
[Add and retrieve objects from hello cache]: #add-object
[Specify hello expiration of an object in hello cache]: #specify-expiration
[Store ASP.NET session state in hello cache]: #store-session


<!-- IMAGES -->


[StackExchangeNuget]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-stackexchange-redis.png

[NuGetMenu]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-nuget-menu.png

[CacheProperties]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-properties.png

[ManageKeys]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-manage-keys.png

[SessionStateNuGet]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-session-state-provider.png

[BrowseCaches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-browse-caches.png

[Caches]: ./media/cache-dotnet-how-to-use-azure-redis-cache/redis-cache-caches.png







<!-- LINKS -->
[http://redis.io/clients]: http://redis.io/clients
[Develop in other languages for Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn690470.aspx
[How tooretrieve an Azure Redis connection string and use it with Redsmin]: https://redsmin.uservoice.com/knowledgebase/articles/485711-how-to-connect-redsmin-to-azure-redis-cache
[Azure Redis Session State Provider]: http://go.microsoft.com/fwlink/?LinkId=398249
[How to: Configure a Cache Client Programmatically]: http://msdn.microsoft.com/library/windowsazure/gg618003.aspx
[Session State Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320835
[Azure AppFabric Cache: Caching Session State]: http://www.microsoft.com/showcase/details.aspx?uuid=87c833e9-97a9-42b2-8bb1-7601f9b5ca20
[Output Cache Provider for Azure Cache]: http://go.microsoft.com/fwlink/?LinkId=320837
[Azure Shared Caching]: http://msdn.microsoft.com/library/windowsazure/gg278356.aspx
[Team Blog]: http://blogs.msdn.com/b/windowsazure/
[Azure Caching]: http://www.microsoft.com/showcase/Search.aspx?phrase=azure+caching
[How tooConfigure Virtual Machine Sizes]: http://go.microsoft.com/fwlink/?LinkId=164387
[Azure Caching Capacity Planning Considerations]: http://go.microsoft.com/fwlink/?LinkId=320167
[Azure Caching]: http://go.microsoft.com/fwlink/?LinkId=252658
[How to: Set hello Cacheability of an ASP.NET Page Declaratively]: http://msdn.microsoft.com/library/zd1ysf1y.aspx
[How to: Set a Page's Cacheability Programmatically]: http://msdn.microsoft.com/library/z852zf6b.aspx
[Configure a cache in Azure Redis Cache]: http://msdn.microsoft.com/library/azure/dn793612.aspx

[StackExchange.Redis configuration model]: https://stackexchange.github.io/StackExchange.Redis/Configuration

[Work with .NET objects in hello cache]: http://msdn.microsoft.com/library/dn690521.aspx#Objects


[NuGet Package Manager Installation]: http://go.microsoft.com/fwlink/?LinkId=240311
[Cache Pricing Details]: http://www.windowsazure.com/pricing/details/cache/
[Azure portal]: https://portal.azure.com/

[Overview of Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=320830
[Azure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=398247

[Migrate tooAzure Redis Cache]: http://go.microsoft.com/fwlink/?LinkId=317347
[Azure Redis Cache Samples]: http://go.microsoft.com/fwlink/?LinkId=320840
[Using Resource groups toomanage your Azure resources]: ../azure-resource-manager/resource-group-overview.md

[StackExchange.Redis]: http://github.com/StackExchange/StackExchange.Redis
[StackExchange.Redis cache client documentation]: http://github.com/StackExchange/StackExchange.Redis#documentation

[Redis]: http://redis.io/documentation
[Redis data types]: http://redis.io/topics/data-types
[a fifteen minute introduction tooRedis data types]: http://redis.io/topics/data-types-intro

[How Application Strings and Connection Strings Work]: http://azure.microsoft.com/blog/2013/07/17/windows-azure-web-sites-how-application-strings-and-connection-strings-work/


