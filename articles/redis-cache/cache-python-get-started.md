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
# <a name="how-toouse-azure-redis-cache-with-python"></a><span data-ttu-id="99893-103">Hur toouse Azure Redis-Cache med Python</span><span class="sxs-lookup"><span data-stu-id="99893-103">How toouse Azure Redis Cache with Python</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="99893-104">.NET</span><span class="sxs-lookup"><span data-stu-id="99893-104">.NET</span></span>](cache-dotnet-how-to-use-azure-redis-cache.md)
> * [<span data-ttu-id="99893-105">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="99893-105">ASP.NET</span></span>](cache-web-app-howto.md)
> * [<span data-ttu-id="99893-106">Node.js</span><span class="sxs-lookup"><span data-stu-id="99893-106">Node.js</span></span>](cache-nodejs-get-started.md)
> * [<span data-ttu-id="99893-107">Java</span><span class="sxs-lookup"><span data-stu-id="99893-107">Java</span></span>](cache-java-get-started.md)
> * [<span data-ttu-id="99893-108">Python</span><span class="sxs-lookup"><span data-stu-id="99893-108">Python</span></span>](cache-python-get-started.md)
> 
> 

<span data-ttu-id="99893-109">Det här avsnittet visar hur tooget igång med Azure Redis-Cache med hjälp av Python.</span><span class="sxs-lookup"><span data-stu-id="99893-109">This topic shows you how tooget started with Azure Redis Cache using Python.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="99893-110">Krav</span><span class="sxs-lookup"><span data-stu-id="99893-110">Prerequisites</span></span>
<span data-ttu-id="99893-111">Installera [redis-py](https://github.com/andymccurdy/redis-py).</span><span class="sxs-lookup"><span data-stu-id="99893-111">Install [redis-py](https://github.com/andymccurdy/redis-py).</span></span>

## <a name="create-a-redis-cache-on-azure"></a><span data-ttu-id="99893-112">Skapa en Redis-cache på Azure</span><span class="sxs-lookup"><span data-stu-id="99893-112">Create a Redis cache on Azure</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-create.md)]

## <a name="retrieve-hello-host-name-and-access-keys"></a><span data-ttu-id="99893-113">Hämta hello värden och nycklar</span><span class="sxs-lookup"><span data-stu-id="99893-113">Retrieve hello host name and access keys</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-access-keys.md)]

## <a name="enable-hello-non-ssl-endpoint"></a><span data-ttu-id="99893-114">Aktivera hello icke-SSL-slutpunkten</span><span class="sxs-lookup"><span data-stu-id="99893-114">Enable hello non-SSL endpoint</span></span>
<span data-ttu-id="99893-115">Vissa Redis-klienter stöder inte SSL, och som standard hello [icke-SSL-porten är inaktiverad för den nya Azure Redis-Cache instanser](cache-configure.md#access-ports).</span><span class="sxs-lookup"><span data-stu-id="99893-115">Some Redis clients don't support SSL, and by default hello [non-SSL port is disabled for new Azure Redis Cache instances](cache-configure.md#access-ports).</span></span> <span data-ttu-id="99893-116">När hello detta skrivs, hello [redis-kopiera](https://github.com/andymccurdy/redis-py) klienten stöder SSL.</span><span class="sxs-lookup"><span data-stu-id="99893-116">At hello time of this writing, hello [redis-py](https://github.com/andymccurdy/redis-py) client doesn't support SSL.</span></span> 

[!INCLUDE [redis-cache-create](../../includes/redis-cache-non-ssl-port.md)]

## <a name="add-something-toohello-cache-and-retrieve-it"></a><span data-ttu-id="99893-117">Lägga till något toohello cache-lagra och hämta det</span><span class="sxs-lookup"><span data-stu-id="99893-117">Add something toohello cache and retrieve it</span></span>
    >>> import redis
    >>> r = redis.StrictRedis(host='<name>.redis.cache.windows.net',
          port=6380, db=0, password='<key>', ssl=True)
    >>> r.set('foo', 'bar')
    True
    >>> r.get('foo')
    b'bar'


<span data-ttu-id="99893-118">Ersätt `<name>` med ditt cachenamn och `key` med din åtkomstnyckel.</span><span class="sxs-lookup"><span data-stu-id="99893-118">Replace `<name>` with your cache name and `key` with your access key.</span></span>

<!--Image references-->
[1]: ./media/cache-python-get-started/redis-cache-new-cache-menu.png
[2]: ./media/cache-python-get-started/redis-cache-cache-create.png
