---
title: "Så här använder du Azure Redis Cache med Node.js | Microsoft Docs"
description: "Kom igång med Azure Redis Cache med hjälp av Node.js och node_redis."
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: v-lincan
ms.assetid: 06fddc95-8029-4a8d-83f5-ebd5016891d9
ms.service: cache
ms.devlang: nodejs
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: wesmc
ms.openlocfilehash: e8b4818ad2b876ce4d55cf8aa2aa0e0162dea963
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-use-azure-redis-cache-with-nodejs"></a>Så här använder du Azure Redis Cache med Node.js
> [!div class="op_single_selector"]
> * [NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Azure Redis Cache ger dig tillgång till en säker och dedikerad Redis-cache som hanteras av Microsoft. Din cache är tillgänglig från alla program i Microsoft Azure.

I det här avsnittet visar vi hur du kommer igång med Azure Redis Cache med Node.js. 

## <a name="prerequisites"></a>Nödvändiga komponenter
Installera [node_redis](https://github.com/mranney/node_redis):

    npm install redis

Den här självstudien använder [node_redis](https://github.com/mranney/node_redis). Exempel på användning av andra Node.js-klienter finns i dokumentationen till enskilda Node.js-klienter som anges i [Node.js Redis-klienter](http://redis.io/clients#nodejs).

## <a name="create-a-redis-cache-on-azure"></a>Skapa en Redis-cache på Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-the-host-name-and-access-keys"></a>Hämta värdnamn och åtkomstnycklar
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="connect-to-the-cache-securely-using-ssl"></a>Ansluta till cache på ett säkert sätt med hjälp av SSL
De senaste build-versionerna av [node_redis](https://github.com/mranney/node_redis) ger stöd för att ansluta till Azure Redis Cache med hjälp av SSL. I följande exempel visas hur du ansluter till Azure Redis Cache med hjälp av SSL-slutpunkten 6380. Ersätt `<name>` med namnet på din cache och `<key>` med antingen en primär eller sekundär nyckel som beskrivs i föregående avsnitt, [Hämta värdnamn och åtkomstnycklar](#retrieve-the-host-name-and-access-keys).

     var redis = require("redis");

      // Add your cache name and access key.
    var client = redis.createClient(6380,'<name>.redis.cache.windows.net', {auth_pass: '<key>', tls: {servername: '<name>.redis.cache.windows.net'}});

> [!NOTE]
> Icke-SSL-porten har inaktiverats för nya Azure Redis Cache-instanser. Om du använder en annan klient som inte har stöd för SSL kan du läsa [Aktivera icke-SSL-porten](cache-configure.md#access-ports) (på engelska).
> 
> 

## <a name="add-something-to-the-cache-and-retrieve-it"></a>Lägg till något i cachen och hämta det
I följande exempel visas hur du ansluter till en Azure Redis Cache-instans och lagrar och hämtar ett objekt från cachen. Fler exempel på hur du använder Redis med [node_redis](https://github.com/mranney/node_redis)-klienten finns på [http://redis.js.org/](http://redis.js.org/).

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
* [Aktivera cachediagnostik](cache-how-to-monitor.md#enable-cache-diagnostics) så att du kan [övervaka](cache-how-to-monitor.md) hälsotillståndet för cacheminnet.
* Läs den officiella [Redis-dokumentationen](http://redis.io/documentation).

