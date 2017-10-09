---
title: aaaHow tooScale Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur tooscale din Azure Redis-Cache instanser"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: sdanie
ms.openlocfilehash: 8d7c015a539f872913056392aa080bf3f445bd03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooscale-azure-redis-cache"></a>Hur tooScale Azure Redis-Cache
Azure Redis-Cache har olika cache erbjudanden, vilket ger flexibilitet i hello valet av cachestorlek och funktioner. När en cache har skapats kan skala du hello storlek och hello prisnivån hello cache ändrar hello kraven i ditt program. Den här artikeln visar hur tooscale ditt cacheminne i hello Azure-portalen och använda verktyg som Azure PowerShell och Azure CLI.

## <a name="when-tooscale"></a>När tooscale
Du kan använda hello [övervakning](cache-how-to-monitor.md) funktioner i Azure Redis-Cache toomonitor hello hälsotillstånd och prestanda för ditt cacheminne och hjälper dem att avgöra när tooscale hello cache. 

Du kan övervaka hello följande mått toohelp Bestäm om du behöver tooscale.

* Redis-serverbelastning
* Minnesanvändning
* Nätverkets bandbredd
* CPU-användning

Om du finner att ditt cacheminne är inte längre uppfyller kraven för ditt program, kan du skala tooa större eller mindre cache prisnivån som passar ditt program. Läs mer om hur du avgör vilket cachelagra prisnivå nivå toouse [vilka Redis-Cache erbjudande och vilken storlek ska jag använda](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Skala en cache
tooscale ditt cacheminne [Bläddra toohello cache](cache-configure.md#configure-redis-cache-settings) i hello [Azure-portalen](https://portal.azure.com) och på **skala** från hello **resurs menyn**.

![Skala](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Välj hello önskade prisnivån från hello **Välj prisnivån** bladet och klicka på **Välj**.

![Prisnivå][redis-cache-pricing-tier-blade]


Du kan skala tooa olika prisnivån med hello följande begränsningar:

* Du kan skala från en högre prisnivå nivå tooa lägre prisnivån.
  * Du kan skala från en **Premium** cache ned tooa **Standard** eller en **grundläggande** cache.
  * Du kan skala från en **Standard** cache ned tooa **grundläggande** cache.
* Du kan skala från en **grundläggande** cachelagra tooa **Standard** cache men du kan inte ändra hello storleken på hello samtidigt. Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärden toohello önskad storlek.
* Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache. Du måste skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** i ett efterföljande skalning åtgärden.
* Du kan skala från en större storlek ned toohello **C0 (250 MB)** storlek.
 
Medan hello cache skalas toohello nya prisnivån, en **skalning** status visas i hello **Redis-Cache** bladet.

![Skalning][redis-cache-scaling]

När skalning är klar hello status ändras från **skalning** för**kör**.

## <a name="how-tooautomate-a-scaling-operation"></a>Hur tooautomate skalning åtgärden
I tillägg tooscaling hello cacheinstanser i Azure-portalen kan du skala med PowerShell-cmdletar, Azure CLI och hello med hjälp av Microsoft Azure Management bibliotek (MAML). 

* [Skala med hjälp av PowerShell](#scale-using-powershell)
* [Skala med Azure CLI](#scale-using-azure-cli)
* [Skala med MAML](#scale-using-maml)

### <a name="scale-using-powershell"></a>Skala med hjälp av PowerShell
Du kan skala dina Azure Redis-Cache-instanser med PowerShell med hjälp av hello [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet när hello `Size`, `Sku`, eller `ShardCount` ändras egenskaperna. hello följande exempel visas hur tooscale en cache med namnet `myCache` tooa 2,5 GB cache. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Läs mer om att skala med PowerShell [tooscale ett Redis-cache med hjälp av Powershell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Skala med Azure CLI
tooscale din Azure Redis-Cache-instanser som använder Azure CLI, anropa hello `azure rediscache set` kommando- och pass i hello önskad konfigurationsändringar som innehåller en ny storlek eller sku klusterstorleken, beroende på hello önskad skalning igen.

Mer information om att skala med Azure CLI finns [ändra inställningar för en befintlig Redis-Cache](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Skala med MAML
tooscale din Azure Redis-Cache-instanser som använder hello [Microsoft Azure Management bibliotek (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), anrop hello `IRedisOperations.CreateOrUpdate` metod och skicka hello nya storleken för hello `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting hello access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // tooscale, set a new size for hello redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Mer information finns i hello [hantera Redis-Cache med MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exempel.

## <a name="scaling-faq"></a>Skalning vanliga frågor och svar
hello innehåller följande lista svar toocommonly frågor och svar om Azure Redis-Cache skalning.

* [Kan jag skala till, från eller inom en Premium-cache?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Efter skalning finns det toochange Mina cache namn eller åtkomst nycklar?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Hur fungerar skalning?](#how-does-scaling-work)
* [Jag förlorar data från min cache under skalning?](#will-i-lose-data-from-my-cache-during-scaling)
* [Är mina egna databaser inställningen påverkas under skalning?](#is-my-custom-databases-setting-affected-during-scaling)
* [Kommer min cache vara tillgängliga under skalning?](#will-my-cache-be-available-during-scaling)
* [Åtgärder som inte stöds](#operations-that-are-not-supported)
* [Hur lång tid skalning tar?](#how-long-does-scaling-take)
* [Hur vet jag när skalning är klar?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Kan jag skala till, från eller inom en Premium-cache?
* Du kan skala från en **Premium** cache ned tooa **grundläggande** eller **Standard** prisnivån.
* Du kan skala från en **Premium** cache priser nivå tooanother.
* Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache. Du måste först skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** vid en senare skalning åtgärden.
* Om du har aktiverat kluster när du skapade din **Premium** cache, kan du [ändra hello klusterstorleken](cache-how-to-premium-clustering.md#cluster-size). Om ditt cacheminne har skapats utan kluster aktiverad, kan du inte konfigurera klustring vid ett senare tillfälle.
  
  Mer information finns i [hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-toochange-my-cache-name-or-access-keys"></a>Efter skalning finns det toochange Mina cache namn eller åtkomst nycklar?
Nej, cache namn och nycklar är oförändrade under en åtgärd med skalning.

### <a name="how-does-scaling-work"></a>Hur fungerar skalning?
* När en **grundläggande** cache skalas tooa olika storlek, den är avstängd och ett nytt cacheminne har etablerats med hello nya storleken. Under denna tid hello cache är inte tillgänglig och alla data i hello cacheminnet har gått förlorade.
* När en **grundläggande** cache är skalade tooa **Standard** cache, en replik-cache är etablerad och hello data kopieras från hello primära cache toohello replik cache. hello cache finns kvar under hello skalning processen.
* När en **Standard** cache är skalade tooa annan storlek eller tooa **Premium** cache, en hello repliker är avstängd och nytt etableras toohello nya storlek och hello data överförs och hello andra replik utför en växling vid fel innan etableras igen, liknande toohello process som sker vid fel på en hello cache-noder.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Jag förlorar data från min cache under skalning?
* När en **grundläggande** cache är skalade tooa nya storlek, alla data går förlorad och hello cachen inte är tillgänglig under hello skalning igen.
* När en **grundläggande** cache är skalade tooa **Standard** cache, hello data i cacheminnet hello vanligtvis bevaras.
* När en **Standard** cache är skalade tooa större storlek eller nivån eller en **Premium** cache skalas tooa större storlek, alla data bevaras normalt. Vid skalning en **Standard** eller **Premium** cache ned tooa mindre storlek, data kan gå förlorade beroende på hur mycket data finns i cacheminnet för hello relaterade toohello nya storleken när skalas. Om data förloras vid skalning nycklar avlägsnas med hello [allkeys lru](http://redis.io/topics/lru-cache) av principen. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Är mina egna databaser inställningen påverkas under skalning?
Vissa prisnivåer har olika [databaser gränser](cache-configure.md#databases), så det finns vissa saker när skala ned om du konfigurerat ett anpassat värde för hello `databases` anger under Skapa cache.

* När skalning tooa prisnivån med en lägre `databases` gränsen än hello aktuella nivån:
  * Om du använder hello standardantalet `databases` som är 16 för alla prisnivåer inga data går förlorade.
  * Om du använder ett anpassat antal `databases` som faller inom hello gränser för hello nivå toowhich du skalning, detta `databases` inställningen bevaras och inga data går förlorade.
  * Om du använder ett anpassat antal `databases` som överskrider hello gränserna för hello ny nivå, hello `databases` inställningen är nedsänkt toohello gränserna för hello ny nivå och alla data i hello bort databaser har gått förlorade.
* När skalning tooa prisnivån med hello samma eller högre `databases` gränsen än hello aktuella nivån din `databases` inställningen bevaras och inga data går förlorade.

Observera att Standard och Premium har ett SLA för 99,9% för tillgänglighet, men det finns inga SLA för förlust av data.

### <a name="will-my-cache-be-available-during-scaling"></a>Kommer min cache vara tillgängliga under skalning?
* **Standard** och **Premium** cacheminnen vara tillgängliga under hello skalning igen.
* **Grundläggande** cacheminnen offline under skalning operations tooa olika storlek, men fortfarande är tillgängliga vid skalning från **grundläggande** för**Standard**.

### <a name="operations-that-are-not-supported"></a>Åtgärder som inte stöds
* Du kan skala från en högre prisnivå nivå tooa lägre prisnivån.
  * Du kan skala från en **Premium** cache ned tooa **Standard** eller en **grundläggande** cache.
  * Du kan skala från en **Standard** cache ned tooa **grundläggande** cache.
* Du kan skala från en **grundläggande** cachelagra tooa **Standard** cache men du kan inte ändra hello storleken på hello samtidigt. Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärden toohello önskad storlek.
* Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache. Du måste först skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** vid en senare skalning åtgärden.
* Du kan skala från en större storlek ned toohello **C0 (250 MB)** storlek.

Om en skalning misslyckas hello tjänsten kommer försök toorevert hello och hello cache återställs toohello ursprungliga storleken.

### <a name="how-long-does-scaling-take"></a>Hur lång tid skalning tar?
Skalning tar cirka 20 minuter beroende på hur mycket data är i hello-cachen.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Hur vet jag när skalning är klar?
Du kan se hello skalning åtgärden pågår i hello Azure-portalen. När skalning är klar hello status hello cache ändras för**kör**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



