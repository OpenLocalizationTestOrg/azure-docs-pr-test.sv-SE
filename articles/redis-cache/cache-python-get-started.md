---
title: aaaHow toouse Azure Redis-Cache med Python | Microsoft Docs
description: "Kom igång med Azure Redis Cache med Python"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: v-lincan
ms.assetid: f186202c-fdad-4398-af8c-aee91ec96ba3
ms.service: cache
ms.devlang: python
ms.topic: hero-article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 02/10/2017
ms.author: sdanie
ms.openlocfilehash: 74c03eb4ce17ff3574595fd2bb37e399d71c6eb4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-azure-redis-cache-with-python"></a>Hur toouse Azure Redis-Cache med Python
> [!div class="op_single_selector"]
> * [.NET](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [ASP.NET](cache-web-app-howto.md)
> * [Node.js](cache-nodejs-get-started.md)
> * [Java](cache-java-get-started.md)
> * [Python](cache-python-get-started.md)
> 
> 

Det här avsnittet visar hur tooget igång med Azure Redis-Cache med hjälp av Python.

## <a name="prerequisites"></a>Krav
Installera [redis-py](https://github.com/andymccurdy/redis-py).

## <a name="create-a-redis-cache-on-azure"></a>Skapa en Redis-cache på Azure
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a>Hämta hello värden och nycklar
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-hello-non-ssl-endpoint"></a>Aktivera hello icke-SSL-slutpunkten
Vissa Redis-klienter stöder inte SSL, och som standard hello [icke-SSL-porten är inaktiverad för den nya Azure Redis-Cache instanser](cache-configure.md#access-ports). När hello detta skrivs, hello [redis-kopiera](https://github.com/andymccurdy/redis-py) klienten stöder SSL. 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-toohello-cache-and-retrieve-it"></a>Lägga till något toohello cache-lagra och hämta det
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


Ersätt `<name>` med ditt cachenamn och `key` med din åtkomstnyckel.

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
