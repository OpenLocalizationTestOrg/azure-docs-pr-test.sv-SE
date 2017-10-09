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
# <a name="how-toouse-azure-redis-cache-with-java"></a>Hur toouse Azure Redis-Cache med Java
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis-Cache ger du åtkomst till dedikerad tooa Redis-cache, hanteras av Microsoft. Din cache är tillgänglig från alla program i Microsoft Azure.

Det här avsnittet visar hur tooget igång med Azure Redis-Cache med Java.

## <a name="prerequisites"></a>Krav
[Jedis](https://github.com/xetorthio/jedis) – Java-klient för Redis

Den här självstudien använder Jedis, men du kan använda valfri Java-klient som finns i [http://redis.io/clients](http://redis.io/clients).

## <a name="create-a-redis-cache-on-azure"></a>Skapa en Redis-cache på Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Hämta hello värden och nycklar
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a>Ansluta toohello cache på ett säkert sätt med SSL
hello senaste versioner av [jedis](https://github.com/xetorthio/jedis) ger stöd för att ansluta tooAzure Redis-Cache med hjälp av SSL. hello som följande exempel visar hur tooconnect tooAzure Redis-Cache med hello SSL-slutpunkten för 6380. Ersätt `<name>` med hello namnet på ditt cacheminne och `<key>` med antingen primära och sekundära nycklarna enligt beskrivningen i hello tidigare [hämta hello namn och värdnycklar](#retrieve-the-host-name-and-access-keys) avsnitt.

    boolean useSsl = true;
    /* In this line, replace <name> with your cache name: */
    JedisShardInfo shardInfo = new JedisShardInfo("<name>.redis.cache.windows.net", 6380, useSsl);
    shardInfo.setPassword("<key>"); /* Use your access key. */

> [!NOTE]
> hello icke-SSL-porten är inaktiverad för den nya Azure Redis-Cache instanser. Om du använder en annan klient som inte stöder SSL, se [hur tooenable hello icke-SSL-porten](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Lägga till något toohello cache-lagra och hämta det
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


## <a name="next-steps"></a>Nästa steg
* [Aktivera cache diagnostik](https://msdn.microsoft.com/library/azure/dn763945.aspx#EnableDiagnostics) så att du kan [övervakaren](https://msdn.microsoft.com/library/azure/dn763945.aspx) hello hälsotillståndet för ditt cacheminne.
* Läs hello officiella [Redis dokumentationen](http://redis.io/documentation).
