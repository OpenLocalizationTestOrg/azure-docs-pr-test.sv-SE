---
title: "Så här skalar du Azure Redis-Cache | Microsoft Docs"
description: "Lär dig hur du skala Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: 
author: wesmc7777
manager: cfowler
editor: 
ms.assetid: 350db214-3b7c-4877-bd43-fef6df2db96c
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 04/11/2017
ms.author: wesmc
ms.openlocfilehash: bee7771c53cfad4a925d5c270569b7a82e45b4d8
ms.sourcegitcommit: 2a70752d0987585d480f374c3e2dba0cd5097880
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-scale-azure-redis-cache"></a>Så här skalar du Azure Redis-Cache
Azure Redis-Cache har olika cache-erbjudanden som ger flexibilitet vid val av cachestorlek och funktioner. När en cache har skapats kan skala du storlek och prisnivå för cachen om kraven för ditt program ändrar. Den här artikeln visar hur du skala ditt cacheminne i Azure-portalen och använda verktyg som Azure PowerShell och Azure CLI.

## <a name="when-to-scale"></a>När ska du skala
Du kan använda den [övervakning](cache-how-to-monitor.md) funktioner i Azure Redis-Cache att övervaka hälsotillstånd och prestanda för ditt cacheminne och hjälper dem att avgöra när skala cachen. 

Du kan övervaka följande väden för att bestämma om du behöver skala.

* Redis-serverbelastning
* Minnesanvändning
* Nätverksbandbredd
* CPU-användning

Om du finner att ditt cacheminne är inte längre uppfyller kraven för ditt program, kan du skala till ett större eller mindre cacheminne prisnivån som passar ditt program. Mer information om hur du avgör vilken prisnivå du använder cache finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

## <a name="scale-a-cache"></a>Skala en cache
Att skala din cache [Bläddra till cachen](cache-configure.md#configure-redis-cache-settings) i den [Azure-portalen](https://portal.azure.com) och på **skala** från den **resurs menyn**.

![Skala](./media/cache-how-to-scale/redis-cache-scale-menu.png)

Välj den önskade prisnivån från den **Välj prisnivån** bladet och klicka på **Välj**.

![Prisnivå][redis-cache-pricing-tier-blade]


Du kan skala till en annan prisnivå med följande begränsningar:

* Du kan skala från en högre prisnivå till en lägre prisnivå.
  * Du kan skala från en **Premium** cachelagra ned till en **Standard** eller en **grundläggande** cache.
  * Du kan skala från en **Standard** cachelagra ned till en **grundläggande** cache.
* Du kan skala från en **grundläggande** cachelagra till en **Standard** cache men du kan inte ändra storlek på samma gång. Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärd till önskad storlek.
* Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache. Du måste skala från **grundläggande** till **Standard** i en skalning åtgärd och sedan från **Standard** till **Premium** i ett efterföljande skalning åtgärden.
* Du kan skala från en större storlek för den **C0 (250 MB)** storlek.
 
När cachen skalas till den nya prisnivån en **skalning** status visas i den **Redis-Cache** bladet.

![Skalning][redis-cache-scaling]

När skalning är klar status ändras från **skalning** till **kör**.

## <a name="how-to-automate-a-scaling-operation"></a>Automatisera en åtgärd för skalning
Förutom att skala din cacheinstanser i Azure-portalen, du kan skala med PowerShell-cmdletar, Azure CLI och genom att använda Microsoft Azure Management bibliotek (MAML). 

* [Skala med hjälp av PowerShell](#scale-using-powershell)
* [Skala med Azure CLI](#scale-using-azure-cli)
* [Skala med MAML](#scale-using-maml)

### <a name="scale-using-powershell"></a>Skala med hjälp av PowerShell
Du kan skala dina Azure Redis-Cache-instanser med PowerShell med hjälp av den [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet när den `Size`, `Sku`, eller `ShardCount` ändras egenskaperna. I följande exempel visas hur du skalar en cache med namnet `myCache` till ett 2,5 GB cacheminne. 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

Läs mer om att skala med PowerShell [att skala ett Redis-cache med hjälp av Powershell](cache-howto-manage-redis-cache-powershell.md#scale).

### <a name="scale-using-azure-cli"></a>Skala med Azure CLI
Om du vill skala din Azure Redis-Cache-instanser som använder Azure CLI anropa den `azure rediscache set` kommando och ange önskade ändringar med en ny storlek, sku eller klusterstorleken, beroende på önskad skalning igen.

Mer information om att skala med Azure CLI finns [ändra inställningar för en befintlig Redis-Cache](cache-manage-cli.md#scale).

### <a name="scale-using-maml"></a>Skala med MAML
Instanser att skala din Azure Redis-Cache med hjälp av den [Microsoft Azure Management bibliotek (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), anropar den `IRedisOperations.CreateOrUpdate` metod och Använd den nya storleken för den `RedisProperties.SKU.Capacity`.

    static void Main(string[] args)
    {
        // For instructions on getting the access token, see
        // https://azure.microsoft.com/documentation/articles/cache-configure/#access-keys
        string token = GetAuthorizationHeader();

        TokenCloudCredentials creds = new TokenCloudCredentials(subscriptionId,token);

        RedisManagementClient client = new RedisManagementClient(creds);
        var redisProperties = new RedisProperties();

        // To scale, set a new size for the redisSKUCapacity parameter.
        redisProperties.Sku = new Sku(redisSKUName,redisSKUFamily,redisSKUCapacity);
        redisProperties.RedisVersion = redisVersion;
        var redisParams = new RedisCreateOrUpdateParameters(redisProperties, redisCacheRegion);
        client.Redis.CreateOrUpdate(resourceGroupName,cacheName, redisParams);
    }

Mer information finns i [hantera Redis-Cache med MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exempel.

## <a name="scaling-faq"></a>Skalning vanliga frågor och svar
I följande lista innehåller svar på vanliga frågor om Azure Redis-Cache skalning.

* [Kan jag skala till, från eller inom en Premium-cache?](#can-i-scale-to-from-or-within-a-premium-cache)
* [Efter skalning måste jag ändra Mina cache namn eller åtkomst nycklar?](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [Hur fungerar skalning?](#how-does-scaling-work)
* [Jag förlorar data från min cache under skalning?](#will-i-lose-data-from-my-cache-during-scaling)
* [Är mina egna databaser inställningen påverkas under skalning?](#is-my-custom-databases-setting-affected-during-scaling)
* [Kommer min cache vara tillgängliga under skalning?](#will-my-cache-be-available-during-scaling)
* [Åtgärder som inte stöds](#operations-that-are-not-supported)
* [Hur lång tid skalning tar?](#how-long-does-scaling-take)
* [Hur vet jag när skalning är klar?](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a>Kan jag skala till, från eller inom en Premium-cache?
* Du kan skala från en **Premium** cachelagra ned till en **grundläggande** eller **Standard** prisnivån.
* Du kan skala från en **Premium** cache prisnivån till en annan.
* Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache. Du måste först skala från **grundläggande** till **Standard** i en skalning åtgärd och sedan från **Standard** till **Premium** vid en senare skalning åtgärden.
* Om du har aktiverat kluster när du skapade din **Premium** cache, kan du [ändra klusterstorleken](cache-how-to-premium-clustering.md#cluster-size). Om ditt cacheminne har skapats utan kluster aktiverad, kan du inte konfigurera klustring vid ett senare tillfälle.
  
  Mer information finns i [Konfigurera klustring för premium Azure Redis-cache](cache-how-to-premium-clustering.md).

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a>Efter skalning måste jag ändra Mina cache namn eller åtkomst nycklar?
Nej, cache namn och nycklar är oförändrade under en åtgärd med skalning.

### <a name="how-does-scaling-work"></a>Hur fungerar skalning?
* När en **grundläggande** cache skalas till en annan storlek, den är avstängd och ett nytt cacheminne etableras med den nya storleken. Cacheminnet är inte tillgänglig under denna tid, och alla data i cacheminnet går förlorade.
* När en **grundläggande** cache skalas till en **Standard** cache, en replik-cache är etablerad och data kopieras från den primära cachen till replik-cachen. Cachen finns kvar under skalning.
* När en **Standard** cache skalas till en annan storlek eller till en **Premium** cache, en av replikerna stängs ner och etableras igen till den nya storleken och de data som överförs och den andra repliken Utför en växling vid fel innan det är nytt etablerade, ungefär på samma sätt som sker vid fel på en cachenoder.

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a>Jag förlorar data från min cache under skalning?
* När en **grundläggande** cache skalas till en ny storlek, alla data går förlorade och cachen är inte tillgänglig under åtgärden skalning.
* När en **grundläggande** cache skalas till en **Standard** bevaras vanligtvis cache, data i cacheminnet.
* När en **Standard** cache skalas till en större storlek eller nivån eller en **Premium** cache skalas till en större storlek, alla data bevaras normalt. Vid skalning en **Standard** eller **Premium** cache ned till en mindre storlek, data kan gå förlorade beroende på hur mycket data finns i cacheminnet för relaterade till den nya storleken när skalas. Om data förloras vid skalning nycklar avlägsnas med hjälp av den [allkeys lru](http://redis.io/topics/lru-cache) av principen. 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a>Är mina egna databaser inställningen påverkas under skalning?
Vissa prisnivåer har olika [databaser gränser](cache-configure.md#databases), så det finns vissa saker när skala ned om du konfigurerat ett anpassat värde för den `databases` anger under Skapa cache.

* Vid skalning till en prisnivå med en lägre `databases` gränsen än den aktuella nivån:
  * Om du använder standardantalet `databases` som är 16 för alla prisnivåer inga data går förlorade.
  * Om du använder ett anpassat antal `databases` som ligger inom gränserna för den nivå som du skalning, detta `databases` inställningen bevaras och inga data går förlorade.
  * Om du använder ett anpassat antal `databases` som överskrider gränserna för ny nivå av `databases` inställningen sänks till gränser för ny nivå och alla data i de borttagna databaserna går förlorade.
* Vid skalning till en prisnivå med samma eller högre `databases` gränsen än den aktuella nivån din `databases` inställningen bevaras och inga data går förlorade.

Observera att Standard och Premium har ett SLA för 99,9% för tillgänglighet, men det finns inga SLA för förlust av data.

### <a name="will-my-cache-be-available-during-scaling"></a>Kommer min cache vara tillgängliga under skalning?
* **Standard** och **Premium** cacheminnen finns kvar under åtgärden skalning.
* **Grundläggande** cacheminnen är offline under skalning åtgärder till en annan storlek men fortfarande är tillgängliga vid skalning från **grundläggande** till **Standard**.

### <a name="operations-that-are-not-supported"></a>Åtgärder som inte stöds
* Du kan skala från en högre prisnivå till en lägre prisnivå.
  * Du kan skala från en **Premium** cachelagra ned till en **Standard** eller en **grundläggande** cache.
  * Du kan skala från en **Standard** cachelagra ned till en **grundläggande** cache.
* Du kan skala från en **grundläggande** cachelagra till en **Standard** cache men du kan inte ändra storlek på samma gång. Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärd till önskad storlek.
* Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache. Du måste först skala från **grundläggande** till **Standard** i en skalning åtgärd och sedan från **Standard** till **Premium** vid en senare skalning åtgärden.
* Du kan skala från en större storlek för den **C0 (250 MB)** storlek.

Om en skalning misslyckas tjänsten kommer att försöka återställa igen och cachen återgår till den ursprungliga storleken.

### <a name="how-long-does-scaling-take"></a>Hur lång tid skalning tar?
Skalning tar cirka 20 minuter beroende på hur mycket data finns i cacheminnet.

### <a name="how-can-i-tell-when-scaling-is-complete"></a>Hur vet jag när skalning är klar?
Du kan se skalning pågår i Azure-portalen. När skalning är klar, status för cachen ändras till **kör**.

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



