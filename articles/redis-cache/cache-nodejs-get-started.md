---
title: "Så här använder du Azure Redis Cache med Node.js | Microsoft Docs"
description: "Kom igång med Azure Redis Cache med hjälp av Node.js och node_redis."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: 530191637b1aa91ee1d7fe5b5bb032c60983f7dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a><span data-ttu-id="7d1c7-103">Så här använder du Azure Redis Cache med Node.js</span><span class="sxs-lookup"><span data-stu-id="7d1c7-103">How to use Azure Redis Cache with Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7d1c7-104">.NET</span><span class="sxs-lookup"><span data-stu-id="7d1c7-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="7d1c7-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7d1c7-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="7d1c7-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="7d1c7-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="7d1c7-107">Java</span><span class="sxs-lookup"><span data-stu-id="7d1c7-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="7d1c7-108">Python</span><span class="sxs-lookup"><span data-stu-id="7d1c7-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="7d1c7-109">Azure Redis Cache ger dig tillgång till en säker och dedikerad Redis-cache som hanteras av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-109">Azure Redis Cache gives you access to a secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="7d1c7-110">Din cache är tillgänglig från alla program i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="7d1c7-111">I det här avsnittet visar vi hur du kommer igång med Azure Redis Cache med Node.js.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-111">This topic shows you how to get started with Azure Redis Cache using Node.js.</span></span> <span data-ttu-id="7d1c7-112">Ett annat exempel på Azure Redis Cache med Node.js finns i [Bygga ett Node.js chattprogram med Socket.IO på en Azure-webbplats](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-112">For another example of using Azure Redis Cache with Node.js, see [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d1c7-113">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="7d1c7-113">Prerequisites</span></span>
<span data-ttu-id="7d1c7-114">Installera [node_redis](https://github.com/mranney/node_redis):</span><span class="sxs-lookup"><span data-stu-id="7d1c7-114">Install [node_redis](https://github.com/mranney/node_redis):</span></span>

    npm install redis

<span data-ttu-id="7d1c7-115">Den här självstudien använder [node_redis](https://github.com/mranney/node_redis).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-115">This tutorial uses [node_redis](https://github.com/mranney/node_redis).</span></span> <span data-ttu-id="7d1c7-116">Exempel på användning av andra Node.js-klienter finns i dokumentationen till enskilda Node.js-klienter som anges i [Node.js Redis-klienter](http://redis.io/clients#nodejs).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-116">For examples of using other Node.js clients, see the individual documentation for the Node.js clients listed at [Node.js Redis clients](http://redis.io/clients#nodejs).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="7d1c7-117">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="7d1c7-117">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a><span data-ttu-id="7d1c7-118">Hämta värdnamn och åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="7d1c7-118">Retrieve the host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a><span data-ttu-id="7d1c7-119">Ansluta till cache på ett säkert sätt med hjälp av SSL</span><span class="sxs-lookup"><span data-stu-id="7d1c7-119">Connect to the cache securely using SSL</span></span>
<span data-ttu-id="7d1c7-120">De senaste build-versionerna av [node_redis](https://github.com/mranney/node_redis) ger stöd för att ansluta till Azure Redis Cache med hjälp av SSL.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-120">The latest builds of [node_redis](https://github.com/mranney/node_redis) provide support for connecting to Azure Redis Cache using SSL.</span></span> <span data-ttu-id="7d1c7-121">I följande exempel visas hur du ansluter till Azure Redis Cache med hjälp av SSL-slutpunkten 6380.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-121">The following example shows how to connect to Azure Redis Cache using the SSL endpoint of 6380.</span></span> <span data-ttu-id="7d1c7-122">Ersätt `<name>` med namnet på din cache och `<key>` med antingen en primär eller sekundär nyckel som beskrivs i föregående avsnitt, [Hämta värdnamn och åtkomstnycklar](#retrieve-the-host-name-and-access-keys).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-122">Replace `<name>` with the name of your cache and `<key>` with either your primary or secondary key as described in the previous [Retrieve the host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> <span data-ttu-id="7d1c7-123">Icke-SSL-porten har inaktiverats för nya Azure Redis Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-123">The non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="7d1c7-124">Om du använder en annan klient som inte har stöd för SSL kan du läsa [Aktivera icke-SSL-porten](cache-configure.md#access-ports) (på engelska).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-124">If you are using a different client that doesn't support SSL, see [How to enable the non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a><span data-ttu-id="7d1c7-125">Lägg till något i cachen och hämta det</span><span class="sxs-lookup"><span data-stu-id="7d1c7-125">Add something to the cache and retrieve it</span></span>
<span data-ttu-id="7d1c7-126">I följande exempel visas hur du ansluter till en Azure Redis Cache-instans och lagrar och hämtar ett objekt från cachen.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-126">The following example shows you how to connect to an Azure Redis Cache instance, and store and retrieve an item from the cache.</span></span> <span data-ttu-id="7d1c7-127">Fler exempel på hur du använder Redis med [node_redis](https://github.com/mranney/node_redis)-klienten finns på [http://redis.js.org/](http://redis.js.org/).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-127">For more examples of using Redis with the [node_redis](https://github.com/mranney/node_redis) client, see [http://redis.js.org/](http://redis.js.org/).</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

<span data-ttu-id="7d1c7-128">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7d1c7-128">Output:</span></span>

    OK
    value


## <a name="next-steps"></a><span data-ttu-id="7d1c7-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d1c7-129">Next steps</span></span>
* <span data-ttu-id="7d1c7-130">[Aktivera cachediagnostik](cache-how-to-monitor.md#enable-cache-diagnostics) så att du kan [övervaka](cache-how-to-monitor.md) hälsotillståndet för cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="7d1c7-130">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) the health of your cache.</span></span>
* <span data-ttu-id="7d1c7-131">Läs den officiella [Redis-dokumentationen](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="7d1c7-131">Read the official [Redis documentation](http://redis.io/documentation).</span></span>

