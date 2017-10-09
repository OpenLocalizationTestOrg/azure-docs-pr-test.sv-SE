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
# <a name="how-toouse-azure-redis-cache-with-nodejs"></a>Hur toouse Azure Redis-Cache med Node.js
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis-Cache ger du åtkomst tooa säker, dedikerad Redis-cache, hanteras av Microsoft. Din cache är tillgänglig från alla program i Microsoft Azure.

Det här avsnittet visar hur tooget igång med Azure Redis-Cache med hjälp av Node.js. Ett annat exempel på Azure Redis Cache med Node.js finns i [Bygga ett Node.js chattprogram med Socket.IO på en Azure-webbplats](../app-service-web/web-sites-nodejs-chat-app-socketio.md).

## <a name="prerequisites"></a>Förutsättningar
Installera [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Den här självstudien använder [node_redis](https://github.com/mranney/node_redis). Exempel på användning av andra Node.js-klienter finns i hello enskilda dokumentationen för hello Node.js klienter som nämns i [Node.js Redis-klienter](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Skapa en Redis-cache på Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Hämta hello värden och nycklar
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-toohello-cache-securely-using-ssl"></a>Ansluta toohello cache på ett säkert sätt med SSL
hello senaste versioner av [node_redis](https://github.com/mranney/node_redis) ger stöd för att ansluta tooAzure Redis-Cache med hjälp av SSL. hello som följande exempel visar hur tooconnect tooAzure Redis-Cache med hello SSL-slutpunkten för 6380. Ersätt `<name>` med hello namnet på ditt cacheminne och `<key>` med antingen primära och sekundära nycklarna enligt beskrivningen i hello tidigare [hämta hello namn och värdnycklar](#retrieve-the-host-name-and-access-keys) avsnitt.

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> hello icke-SSL-porten är inaktiverad för den nya Azure Redis-Cache instanser. Om du använder en annan klient som inte stöder SSL, se [hur tooenable hello icke-SSL-porten](cache-configure.md#access-ports).
> 
> 

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Lägga till något toohello cache-lagra och hämta det
följande exempel visar du hur tooconnect tooan Azure Redis-Cache-instansen och lagra och hämta ett objekt från cache hello hello. Fler exempel på med hello Redis [node_redis](https://github.com/mranney/node_redis) -klient finns [http://redis.js.org/](http://redis.js.org/).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

    client.set("key1", "value", function(err, reply) {
            console.log(reply);
        });

    client.get("key1",  function(err, reply) {
            console.log(reply);
        });

Resultat:

    OK
    value


## <a name="next-steps"></a>Nästa steg
* [Aktivera cache diagnostik](cache-how-to-monitor.md#enable-cache-diagnostics) så att du kan [övervakaren](cache-how-to-monitor.md) hello hälsotillståndet för ditt cacheminne.
* Läs hello officiella [Redis dokumentationen](http://redis.io/documentation).

