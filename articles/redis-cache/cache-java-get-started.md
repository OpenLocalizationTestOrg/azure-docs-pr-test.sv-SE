---
title: aaaHow toouse Azure Redis-Cache med Java | Microsoft Docs
description: "Kom igång med Azure Redis Cache med Java"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 29275a5e-2e39-4ef2-804f-7ecc5161eab9
ms.service: cache
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 04/13/2017
ms.author: sdanie
ms.openlocfilehash: 7768e879d71f61585b59cf4bd6634ba3f12e001d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-java"></a><span data-ttu-id="69177-103">Hur toouse Azure Redis-Cache med Java</span><span class="sxs-lookup"><span data-stu-id="69177-103">How toouse Azure Redis Cache with Java</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="69177-104">.NET</span><span class="sxs-lookup"><span data-stu-id="69177-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="69177-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="69177-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="69177-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="69177-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="69177-107">Java</span><span class="sxs-lookup"><span data-stu-id="69177-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="69177-108">Python</span><span class="sxs-lookup"><span data-stu-id="69177-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="69177-109">Azure Redis-Cache ger du åtkomst till dedikerad tooa Redis-cache, hanteras av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="69177-109">Azure Redis Cache gives you access tooa dedicated Redis cache, managed by Microsoft.</span></span> <span data-ttu-id="69177-110">Din cache är tillgänglig från alla program i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="69177-110">Your cache is accessible from any application within Microsoft Azure.</span></span>

<span data-ttu-id="69177-111">Det här avsnittet visar hur tooget igång med Azure Redis-Cache med Java.</span><span class="sxs-lookup"><span data-stu-id="69177-111">This topic shows you how tooget started with Azure Redis Cache using Java.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="69177-112">Krav</span><span class="sxs-lookup"><span data-stu-id="69177-112">Prerequisites</span></span>
<span data-ttu-id="69177-113">[Jedis](https://github.com/xetorthio/jedis) – Java-klient för Redis</span><span class="sxs-lookup"><span data-stu-id="69177-113">[Jedis](https://github.com/xetorthio/jedis) - Java client for Redis</span></span>

<span data-ttu-id="69177-114">Den här självstudien använder Jedis, men du kan använda valfri Java-klient som finns i [http://redis.io/clients](http://redis.io/clients).</span><span class="sxs-lookup"><span data-stu-id="69177-114">This tutorial uses Jedis, but you can use any Java client listed at [http://redis.io/clients](http://redis.io/clients).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="69177-115">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="69177-115">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="69177-116">Hämta hello värden och nycklar</span><span class="sxs-lookup"><span data-stu-id="69177-116">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a><span data-ttu-id="69177-117">Ansluta toohello cache på ett säkert sätt med SSL</span><span class="sxs-lookup"><span data-stu-id="69177-117">Connect toohello cache securely using SSL</span></span>
<span data-ttu-id="69177-118">hello senaste versioner av [jedis](https://github.com/xetorthio/jedis) ger stöd för att ansluta tooAzure Redis-Cache med hjälp av SSL.</span><span class="sxs-lookup"><span data-stu-id="69177-118">hello latest builds of [jedis](https://github.com/xetorthio/jedis) provide support for connecting tooAzure Redis Cache using SSL.</span></span> <span data-ttu-id="69177-119">hello som följande exempel visar hur tooconnect tooAzure Redis-Cache med hello SSL-slutpunkten för 6380.</span><span class="sxs-lookup"><span data-stu-id="69177-119">hello following example shows how tooconnect tooAzure Redis Cache using hello SSL endpoint of 6380.</span></span> <span data-ttu-id="69177-120">Ersätt `<name>` med hello namnet på ditt cacheminne och `<key>` med antingen primära och sekundära nycklarna enligt beskrivningen i hello tidigare [hämta hello namn och värdnycklar](#retrieve-the-host-name-and-access-keys) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="69177-120">Replace `<name>` with hello name of your cache and `<key>` with either your primary or secondary key as described in hello previous [Retrieve hello host name and access keys](#retrieve-the-host-name-and-access-keys) section.</span></span>

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> <span data-ttu-id="69177-121">hello icke-SSL-porten är inaktiverad för den nya Azure Redis-Cache instanser.</span><span class="sxs-lookup"><span data-stu-id="69177-121">hello non-SSL port is disabled for new Azure Redis Cache instances.</span></span> <span data-ttu-id="69177-122">Om du använder en annan klient som inte stöder SSL, se [hur tooenable hello icke-SSL-porten](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="69177-122">If you are using a different client that doesn't support SSL, see [How tooenable hello non-SSL port](cache-configure.md#access-ports).</span></span>
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="69177-123">Lägga till något toohello cache-lagra och hämta det</span><span class="sxs-lookup"><span data-stu-id="69177-123">Add something toohello cache and retrieve it</span></span>
    package com.mycompany.app;
    import redis.clients.jedis.Jedis;
    import redis.clients.jedis.JedisShardInfo;

    public class App
    {
      public static void main( String[] args )
      {
        boolean useSsl = true;
        /* In this line, replace <name> with your cache name: */
        JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
        shardInfo.setPassword("<key>"); /* Use your access key. */
        Jedis jedis = new Jedis(shardInfo);
        jedis.set("foo", "bar");
        String value = jedis.get("foo");
      }
    }


## <a name="next-steps"></a><span data-ttu-id="69177-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="69177-124">Next steps</span></span>
* <span data-ttu-id="69177-125">[Aktivera cache diagnostik](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) så att du kan [övervakaren](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello hälsotillståndet för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="69177-125">[Enable cache diagnostics](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) so you can [monitor](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello health of your cache.</span></span>
* <span data-ttu-id="69177-126">Läs hello officiella [Redis dokumentationen](http://redis.io/documentation).</span><span class="sxs-lookup"><span data-stu-id="69177-126">Read hello official [Redis documentation](http://redis.io/documentation).</span></span>
