---
title: aaaHow toouse Azure Redis-Cache med Node.js | Microsoft Docs
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
ms.openlocfilehash: dc8732041d2c4e5793e684e0c80b87a1c9d17f34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-nodejs"></a><span data-ttu-id="7f02d-103">Hur toouse Azure Redis-Cache med Node.js</span><span class="sxs-lookup"><span data-stu-id="7f02d-103">How toouse Azure Redis Cache with Node.js</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7f02d-104">.NET</span><span class="sxs-lookup"><span data-stu-id="7f02d-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="7f02d-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="7f02d-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="7f02d-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="7f02d-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="7f02d-107">Java</span><span class="sxs-lookup"><span data-stu-id="7f02d-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="7f02d-108">Python</span><span class="sxs-lookup"><span data-stu-id="7f02d-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="7f02d-109">Azure Redis-Cache ger du åtkomst tooa säker, dedikerad Redis-cache, hanteras av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="7f02d-109">Azure Redis Cache gives you access tooa secure, dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="7f02d-110">Din cache är tillgänglig från alla program i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="7f02d-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="7f02d-111">Det här avsnittet visar hur tooget igång med Azure Redis-Cache med hjälp av Node.js.</span><span class="sxs-lookup"><span data-stu-id="7f02d-111">This topic shows you how tooget started with Azure Redis Cache using Node.js.</span></span> <span data-ttu-id="7f02d-112">Ett annat exempel på Azure Redis Cache med Node.js finns i [Bygga ett Node.js chattprogram med Socket.IO på en Azure-webbplats](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span><span class="sxs-lookup"><span data-stu-id="7f02d-112">For another example of using Azure Redis Cache with Node.js, see [Build a Node.js Chat Application with Socket.IO on an Azure Website](../app-service-web/web-sites-nodejs-chat-app-socketio.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7f02d-113">Förutsättningar</span><span class="sxs-lookup"><span data-stu-id="7f02d-113">Prerequisites</span></span>
<span data-ttu-id="7f02d-114">Installera [node_redis](https://github.com/mranney/node_redis):</span><span class="sxs-lookup"><span data-stu-id="7f02d-114">Install [node_redis](https://github.com/mranney/node_redis):</span></span>

    npm install redis

<span data-ttu-id="7f02d-115">Den här självstudien använder [node_redis](https://github.com/mranney/node_redis).</span><span class="sxs-lookup"><span data-stu-id="7f02d-115">This tutorial uses [node_redis](https://github.com/mranney/node_redis).</span></span> <span data-ttu-id="7f02d-116">Exempel på användning av andra Node.js-klienter finns i hello enskilda dokumentationen för hello Node.js klienter som nämns i [Node.js Redis-klienter](http://redis.io/clients#nodejs).</span><span class="sxs-lookup"><span data-stu-id="7f02d-116">For examples of using other Node.js clients, see hello individual documentation for hello Node.js clients listed at [Node.js Redis clients](http://redis.io/clients#nodejs).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="7f02d-117">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="7f02d-117">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="7f02d-118">Hämta hello värden och nycklar</span><span class="sxs-lookup"><span data-stu-id="7f02d-118">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a><span data-ttu-id="7f02d-119">Ansluta toohello cache på ett säkert sätt med SSL</span><span class="sxs-lookup"><span data-stu-id="7f02d-119">Connect toohello cache securely using SSL</span></span>
<span data-ttu-id="7f02d-120">hello senaste versioner av [node_redis](https://github.com/mranney/node_redis) ger stöd för att ansluta tooAzure Redis-Cache med hjälp av SSL.</span><span class="sxs-lookup"><span data-stu-id="7f02d-120">hello latest builds of [node_redis](https://github.com/mranney/node_redis) provide support for connecting tooAzure Redis Cache using SSL.</span></span> <span data-ttu-id="7f02d-121">hello som följande exempel visar hur tooconnect tooAzure Redis-Cache med hello SSL-slutpunkten för 6380.</span><span class="sxs-lookup"><span data-stu-id="7f02d-121">hello following example shows how tooconnect tooAzure Redis Cache using hello SSL endpoint of 6380.</span></span> <span data-ttu-id="7f02d-122">Ersätt `<name>` med hello namnet på ditt cacheminne och `<key>` med antingen primära och sekundära nycklarna enligt beskrivningen i hello tidigare [hämta hello namn och värdnycklar](#retrieve-the-host-name-and-access-keys) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7f02d-122">Replace `<name>` with hello name of your cache and `<key>` with either your primary or secondary key as described in hello previous [Retrieve hello host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> <span data-ttu-id="7f02d-123">hello icke-SSL-porten är inaktiverad för den nya Azure Redis-Cache instanser.</span><span class="sxs-lookup"><span data-stu-id="7f02d-123">hello non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="7f02d-124">Om du använder en annan klient som inte stöder SSL, se [hur tooenable hello icke-SSL-porten](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="7f02d-124">If you are using a different client that doesn't support SSL, see [How tooenable hello non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="7f02d-125">Lägga till något toohello cache-lagra och hämta det</span><span class="sxs-lookup"><span data-stu-id="7f02d-125">Add something toohello cache and retrieve it</span></span>
<span data-ttu-id="7f02d-126">följande exempel visar du hur tooconnect tooan Azure Redis-Cache-instansen och lagra och hämta ett objekt från cache hello hello.</span><span class="sxs-lookup"><span data-stu-id="7f02d-126">hello following example shows you how tooconnect tooan Azure Redis Cache instance, and store and retrieve an item from hello cache.</span></span> <span data-ttu-id="7f02d-127">Fler exempel på med hello Redis [node_redis](https://github.com/mranney/node_redis) -klient finns [http://redis.js.org/](http://redis.js.org/).</span><span class="sxs-lookup"><span data-stu-id="7f02d-127">For more examples of using Redis with hello [node_redis](https://github.com/mranney/node_redis) client, see [http://redis.js.org/](http://redis.js.org/).</span></span>

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

<span data-ttu-id="7f02d-128">Resultat:</span><span class="sxs-lookup"><span data-stu-id="7f02d-128">Output:</span></span>

    OK
    value


## <a name="next-steps"></a><span data-ttu-id="7f02d-129">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f02d-129">Next steps</span></span>
* <span data-ttu-id="7f02d-130">[Aktivera cache diagnostik](cache-how-to-monitor.md#enable-cache-diagnostics) så att du kan [övervakaren](cache-how-to-monitor.md) hello hälsotillståndet för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="7f02d-130">[Enable cache diagnostics](cache-how-to-monitor.md#enable-cache-diagnostics) so you can [monitor](cache-how-to-monitor.md) hello health of your cache.</span></span>
* <span data-ttu-id="7f02d-131">Läs hello officiella [Redis dokumentationen](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="7f02d-131">Read hello official [Redis documentation](http://redis.io/documentation).</span></span>

