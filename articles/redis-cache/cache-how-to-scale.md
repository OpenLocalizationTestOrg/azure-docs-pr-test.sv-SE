---
title: "Så här skalar du Azure Redis-Cache | Microsoft Docs"
description: "Lär dig hur du skala Azure Redis-Cache-instanser"
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
ms.openlocfilehash: 91b3580491a1e3504a3891b66606a9bd18c0638f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-scale-azure-redis-cache"></a><span data-ttu-id="71267-103">Så här skalar du Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="71267-103">How to Scale Azure Redis Cache</span></span>
<span data-ttu-id="71267-104">Azure Redis-Cache har olika cache-erbjudanden som ger flexibilitet vid val av cachestorlek och funktioner.</span><span class="sxs-lookup"><span data-stu-id="71267-104">Azure Redis Cache has different cache offerings, which provide flexibility in the choice of cache size and features.</span></span> <span data-ttu-id="71267-105">När en cache har skapats kan skala du storlek och prisnivå för cachen om kraven för ditt program ändrar.</span><span class="sxs-lookup"><span data-stu-id="71267-105">After a cache is created, you can scale the size and the pricing tier of the cache if the requirements of your application change.</span></span> <span data-ttu-id="71267-106">Den här artikeln visar hur du skala ditt cacheminne i Azure-portalen och använda verktyg som Azure PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="71267-106">This article shows you how to scale your cache in both the Azure portal and using tools such as Azure PowerShell and Azure CLI.</span></span>

## <a name="when-to-scale"></a><span data-ttu-id="71267-107">När ska du skala</span><span class="sxs-lookup"><span data-stu-id="71267-107">When to scale</span></span>
<span data-ttu-id="71267-108">Du kan använda den [övervakning](cache-how-to-monitor.md) funktioner i Azure Redis-Cache att övervaka hälsotillstånd och prestanda för ditt cacheminne och hjälper dem att avgöra när skala cachen.</span><span class="sxs-lookup"><span data-stu-id="71267-108">You can use the [monitoring](cache-how-to-monitor.md) features of Azure Redis Cache to monitor the health and performance of your cache and help determine when to scale the cache.</span></span> 

<span data-ttu-id="71267-109">Du kan övervaka följande väden för att bestämma om du behöver skala.</span><span class="sxs-lookup"><span data-stu-id="71267-109">You can monitor the following metrics to help determine if you need to scale.</span></span>

* <span data-ttu-id="71267-110">Redis-serverbelastning</span><span class="sxs-lookup"><span data-stu-id="71267-110">Redis Server Load</span></span>
* <span data-ttu-id="71267-111">Minnesanvändning</span><span class="sxs-lookup"><span data-stu-id="71267-111">Memory Usage</span></span>
* <span data-ttu-id="71267-112">Nätverkets bandbredd</span><span class="sxs-lookup"><span data-stu-id="71267-112">Network Bandwidth</span></span>
* <span data-ttu-id="71267-113">CPU-användning</span><span class="sxs-lookup"><span data-stu-id="71267-113">CPU Usage</span></span>

<span data-ttu-id="71267-114">Om du finner att ditt cacheminne är inte längre uppfyller kraven för ditt program, kan du skala till ett större eller mindre cacheminne prisnivån som passar ditt program.</span><span class="sxs-lookup"><span data-stu-id="71267-114">If you determine that your cache is no longer meeting your application's requirements, you can scale to a larger or smaller cache pricing tier that is right for your application.</span></span> <span data-ttu-id="71267-115">Mer information om hur du avgör vilken prisnivå du använder cache finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="71267-115">For more information on determining which cache pricing tier to use, see [What Redis Cache offering and size should I use](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span></span>

## <a name="scale-a-cache"></a><span data-ttu-id="71267-116">Skala en cache</span><span class="sxs-lookup"><span data-stu-id="71267-116">Scale a cache</span></span>
<span data-ttu-id="71267-117">Att skala din cache [Bläddra till cachen](cache-configure.md#configure-redis-cache-settings) i den [Azure-portalen](https://portal.azure.com) och på **skala** från den **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="71267-117">To scale your cache, [browse to the cache](cache-configure.md#configure-redis-cache-settings) in the [Azure portal](https://portal.azure.com) and click **Scale** from the **Resource menu**.</span></span>

![Skala](./media/cache-how-to-scale/redis-cache-scale-menu.png)

<span data-ttu-id="71267-119">Välj den önskade prisnivån från den **Välj prisnivån** bladet och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="71267-119">Select the desired pricing tier from the **Select pricing tier** blade and click **Select**.</span></span>

![Prisnivå][redis-cache-pricing-tier-blade]


<span data-ttu-id="71267-121">Du kan skala till en annan prisnivå med följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="71267-121">You can scale to a different pricing tier with the following restrictions:</span></span>

* <span data-ttu-id="71267-122">Du kan skala från en högre prisnivå till en lägre prisnivå.</span><span class="sxs-lookup"><span data-stu-id="71267-122">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
  * <span data-ttu-id="71267-123">Du kan skala från en **Premium** cachelagra ned till en **Standard** eller en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-123">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="71267-124">Du kan skala från en **Standard** cachelagra ned till en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-124">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
* <span data-ttu-id="71267-125">Du kan skala från en **grundläggande** cachelagra till en **Standard** cache men du kan inte ändra storlek på samma gång.</span><span class="sxs-lookup"><span data-stu-id="71267-125">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="71267-126">Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärd till önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="71267-126">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
* <span data-ttu-id="71267-127">Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-127">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="71267-128">Du måste skala från **grundläggande** till **Standard** i en skalning åtgärd och sedan från **Standard** till **Premium** i ett efterföljande skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="71267-128">You must scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="71267-129">Du kan skala från en större storlek för den **C0 (250 MB)** storlek.</span><span class="sxs-lookup"><span data-stu-id="71267-129">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>
 
<span data-ttu-id="71267-130">När cachen skalas till den nya prisnivån en **skalning** status visas i den **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="71267-130">While the cache is scaling to the new pricing tier, a **Scaling** status is displayed in the **Redis Cache** blade.</span></span>

![Skalning][redis-cache-scaling]

<span data-ttu-id="71267-132">När skalning är klar status ändras från **skalning** till **kör**.</span><span class="sxs-lookup"><span data-stu-id="71267-132">When scaling is complete, the status changes from **Scaling** to **Running**.</span></span>

## <a name="how-to-automate-a-scaling-operation"></a><span data-ttu-id="71267-133">Automatisera en åtgärd för skalning</span><span class="sxs-lookup"><span data-stu-id="71267-133">How to automate a scaling operation</span></span>
<span data-ttu-id="71267-134">Förutom att skala din cacheinstanser i Azure-portalen, du kan skala med PowerShell-cmdletar, Azure CLI och genom att använda Microsoft Azure Management bibliotek (MAML).</span><span class="sxs-lookup"><span data-stu-id="71267-134">In addition to scaling your cache instances in the Azure portal, you can scale using PowerShell cmdlets, Azure CLI, and by using the Microsoft Azure Management Libraries (MAML).</span></span> 

* [<span data-ttu-id="71267-135">Skala med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="71267-135">Scale using PowerShell</span></span>](#scale-using-powershell)
* [<span data-ttu-id="71267-136">Skala med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="71267-136">Scale using Azure CLI</span></span>](#scale-using-azure-cli)
* [<span data-ttu-id="71267-137">Skala med MAML</span><span class="sxs-lookup"><span data-stu-id="71267-137">Scale using MAML</span></span>](#scale-using-maml)

### <a name="scale-using-powershell"></a><span data-ttu-id="71267-138">Skala med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="71267-138">Scale using PowerShell</span></span>
<span data-ttu-id="71267-139">Du kan skala dina Azure Redis-Cache-instanser med PowerShell med hjälp av den [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet när den `Size`, `Sku`, eller `ShardCount` ändras egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="71267-139">You can scale your Azure Redis Cache instances with PowerShell by using the [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet when the `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> <span data-ttu-id="71267-140">I följande exempel visas hur du skalar en cache med namnet `myCache` till ett 2,5 GB cacheminne.</span><span class="sxs-lookup"><span data-stu-id="71267-140">The following example shows how to scale a cache named `myCache` to a 2.5 GB cache.</span></span> 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="71267-141">Läs mer om att skala med PowerShell [att skala ett Redis-cache med hjälp av Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span><span class="sxs-lookup"><span data-stu-id="71267-141">For more information on scaling with PowerShell, see [To scale a Redis cache using Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span></span>

### <a name="scale-using-azure-cli"></a><span data-ttu-id="71267-142">Skala med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="71267-142">Scale using Azure CLI</span></span>
<span data-ttu-id="71267-143">Om du vill skala din Azure Redis-Cache-instanser som använder Azure CLI anropa den `azure rediscache set` kommando och ange önskade ändringar med en ny storlek, sku eller klusterstorleken, beroende på önskad skalning igen.</span><span class="sxs-lookup"><span data-stu-id="71267-143">To scale your Azure Redis Cache instances using Azure CLI, call the `azure rediscache set` command and pass in the desired configuration changes that include a new size, sku, or cluster size, depending on the desired scaling operation.</span></span>

<span data-ttu-id="71267-144">Mer information om att skala med Azure CLI finns [ändra inställningar för en befintlig Redis-Cache](cache-manage-cli.md#scale).</span><span class="sxs-lookup"><span data-stu-id="71267-144">For more information on scaling with Azure CLI, see [Change settings of an existing Redis Cache](cache-manage-cli.md#scale).</span></span>

### <a name="scale-using-maml"></a><span data-ttu-id="71267-145">Skala med MAML</span><span class="sxs-lookup"><span data-stu-id="71267-145">Scale using MAML</span></span>
<span data-ttu-id="71267-146">Instanser att skala din Azure Redis-Cache med hjälp av den [Microsoft Azure Management bibliotek (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), anropar den `IRedisOperations.CreateOrUpdate` metod och Använd den nya storleken för den `RedisProperties.SKU.Capacity`.</span><span class="sxs-lookup"><span data-stu-id="71267-146">To scale your Azure Redis Cache instances using the [Microsoft Azure Management Libraries (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), call the `IRedisOperations.CreateOrUpdate` method and pass in the new size for the `RedisProperties.SKU.Capacity`.</span></span>

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

<span data-ttu-id="71267-147">Mer information finns i [hantera Redis-Cache med MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exempel.</span><span class="sxs-lookup"><span data-stu-id="71267-147">For more information, see the [Manage Redis Cache using MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample.</span></span>

## <a name="scaling-faq"></a><span data-ttu-id="71267-148">Skalning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="71267-148">Scaling FAQ</span></span>
<span data-ttu-id="71267-149">I följande lista innehåller svar på vanliga frågor om Azure Redis-Cache skalning.</span><span class="sxs-lookup"><span data-stu-id="71267-149">The following list contains answers to commonly asked questions about Azure Redis Cache scaling.</span></span>

* [<span data-ttu-id="71267-150">Kan jag skala till, från eller inom en Premium-cache?</span><span class="sxs-lookup"><span data-stu-id="71267-150">Can I scale to, from, or within a Premium cache?</span></span>](#can-i-scale-to-from-or-within-a-premium-cache)
* [<span data-ttu-id="71267-151">Efter skalning måste jag ändra Mina cache namn eller åtkomst nycklar?</span><span class="sxs-lookup"><span data-stu-id="71267-151">After scaling, do I have to change my cache name or access keys?</span></span>](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [<span data-ttu-id="71267-152">Hur fungerar skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-152">How does scaling work?</span></span>](#how-does-scaling-work)
* [<span data-ttu-id="71267-153">Jag förlorar data från min cache under skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-153">Will I lose data from my cache during scaling?</span></span>](#will-i-lose-data-from-my-cache-during-scaling)
* [<span data-ttu-id="71267-154">Är mina egna databaser inställningen påverkas under skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-154">Is my custom databases setting affected during scaling?</span></span>](#is-my-custom-databases-setting-affected-during-scaling)
* [<span data-ttu-id="71267-155">Kommer min cache vara tillgängliga under skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-155">Will my cache be available during scaling?</span></span>](#will-my-cache-be-available-during-scaling)
* [<span data-ttu-id="71267-156">Åtgärder som inte stöds</span><span class="sxs-lookup"><span data-stu-id="71267-156">Operations that are not supported</span></span>](#operations-that-are-not-supported)
* [<span data-ttu-id="71267-157">Hur lång tid skalning tar?</span><span class="sxs-lookup"><span data-stu-id="71267-157">How long does scaling take?</span></span>](#how-long-does-scaling-take)
* [<span data-ttu-id="71267-158">Hur vet jag när skalning är klar?</span><span class="sxs-lookup"><span data-stu-id="71267-158">How can I tell when scaling is complete?</span></span>](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a><span data-ttu-id="71267-159">Kan jag skala till, från eller inom en Premium-cache?</span><span class="sxs-lookup"><span data-stu-id="71267-159">Can I scale to, from, or within a Premium cache?</span></span>
* <span data-ttu-id="71267-160">Du kan skala från en **Premium** cachelagra ned till en **grundläggande** eller **Standard** prisnivån.</span><span class="sxs-lookup"><span data-stu-id="71267-160">You can't scale from a **Premium** cache down to a **Basic** or **Standard** pricing tier.</span></span>
* <span data-ttu-id="71267-161">Du kan skala från en **Premium** cache prisnivån till en annan.</span><span class="sxs-lookup"><span data-stu-id="71267-161">You can scale from one **Premium** cache pricing tier to another.</span></span>
* <span data-ttu-id="71267-162">Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-162">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="71267-163">Du måste först skala från **grundläggande** till **Standard** i en skalning åtgärd och sedan från **Standard** till **Premium** vid en senare skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="71267-163">You must first scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="71267-164">Om du har aktiverat kluster när du skapade din **Premium** cache, kan du [ändra klusterstorleken](cache-how-to-premium-clustering.md#cluster-size).</span><span class="sxs-lookup"><span data-stu-id="71267-164">If you enabled clustering when you created your **Premium** cache, you can [change the cluster size](cache-how-to-premium-clustering.md#cluster-size).</span></span> <span data-ttu-id="71267-165">Om ditt cacheminne har skapats utan kluster aktiverad, kan du inte konfigurera klustring vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="71267-165">If your cache was created without clustering enabled, you can't configure clustering at a later time.</span></span>
  
  <span data-ttu-id="71267-166">Mer information finns i [Konfigurera klustring för premium Azure Redis-cache](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="71267-166">For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>

### <a name="after-scaling-do-i-have-to-change-my-cache-name-or-access-keys"></a><span data-ttu-id="71267-167">Efter skalning måste jag ändra Mina cache namn eller åtkomst nycklar?</span><span class="sxs-lookup"><span data-stu-id="71267-167">After scaling, do I have to change my cache name or access keys?</span></span>
<span data-ttu-id="71267-168">Nej, cache namn och nycklar är oförändrade under en åtgärd med skalning.</span><span class="sxs-lookup"><span data-stu-id="71267-168">No, your cache name and keys are unchanged during a scaling operation.</span></span>

### <a name="how-does-scaling-work"></a><span data-ttu-id="71267-169">Hur fungerar skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-169">How does scaling work?</span></span>
* <span data-ttu-id="71267-170">När en **grundläggande** cache skalas till en annan storlek, den är avstängd och ett nytt cacheminne etableras med den nya storleken.</span><span class="sxs-lookup"><span data-stu-id="71267-170">When a **Basic** cache is scaled to a different size, it is shut down and a new cache is provisioned using the new size.</span></span> <span data-ttu-id="71267-171">Cacheminnet är inte tillgänglig under denna tid, och alla data i cacheminnet går förlorade.</span><span class="sxs-lookup"><span data-stu-id="71267-171">During this time, the cache is unavailable and all data in the cache is lost.</span></span>
* <span data-ttu-id="71267-172">När en **grundläggande** cache skalas till en **Standard** cache, en replik-cache är etablerad och data kopieras från den primära cachen till replik-cachen.</span><span class="sxs-lookup"><span data-stu-id="71267-172">When a **Basic** cache is scaled to a **Standard** cache, a replica cache is provisioned and the data is copied from the primary cache to the replica cache.</span></span> <span data-ttu-id="71267-173">Cachen finns kvar under skalning.</span><span class="sxs-lookup"><span data-stu-id="71267-173">The cache remains available during the scaling process.</span></span>
* <span data-ttu-id="71267-174">När en **Standard** cache skalas till en annan storlek eller till en **Premium** cache, en av replikerna stängs ner och etableras igen till den nya storleken och de data som överförs och den andra repliken Utför en växling vid fel innan det är nytt etablerade, ungefär på samma sätt som sker vid fel på en cachenoder.</span><span class="sxs-lookup"><span data-stu-id="71267-174">When a **Standard** cache is scaled to a different size or to a **Premium** cache, one of the replicas is shut down and re-provisioned to the new size and the data transferred over, and then the other replica performs a failover before it is re-provisioned, similar to the process that occurs during a failure of one of the cache nodes.</span></span>

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a><span data-ttu-id="71267-175">Jag förlorar data från min cache under skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-175">Will I lose data from my cache during scaling?</span></span>
* <span data-ttu-id="71267-176">När en **grundläggande** cache skalas till en ny storlek, alla data går förlorade och cachen är inte tillgänglig under åtgärden skalning.</span><span class="sxs-lookup"><span data-stu-id="71267-176">When a **Basic** cache is scaled to a new size, all data is lost and the cache is unavailable during the scaling operation.</span></span>
* <span data-ttu-id="71267-177">När en **grundläggande** cache skalas till en **Standard** bevaras vanligtvis cache, data i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="71267-177">When a **Basic** cache is scaled to a **Standard** cache, the data in the cache is typically preserved.</span></span>
* <span data-ttu-id="71267-178">När en **Standard** cache skalas till en större storlek eller nivån eller en **Premium** cache skalas till en större storlek, alla data bevaras normalt.</span><span class="sxs-lookup"><span data-stu-id="71267-178">When a **Standard** cache is scaled to a larger size or tier, or a **Premium** cache is scaled to a larger size, all data is typically preserved.</span></span> <span data-ttu-id="71267-179">Vid skalning en **Standard** eller **Premium** cache ned till en mindre storlek, data kan gå förlorade beroende på hur mycket data finns i cacheminnet för relaterade till den nya storleken när skalas.</span><span class="sxs-lookup"><span data-stu-id="71267-179">When scaling a **Standard** or **Premium** cache down to a smaller size, data may be lost depending on how much data is in the cache related to the new size when it is scaled.</span></span> <span data-ttu-id="71267-180">Om data förloras vid skalning nycklar avlägsnas med hjälp av den [allkeys lru](http://redis.io/topics/lru-cache) av principen.</span><span class="sxs-lookup"><span data-stu-id="71267-180">If data is lost when scaling down, keys are evicted using the [allkeys-lru](http://redis.io/topics/lru-cache) eviction policy.</span></span> 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a><span data-ttu-id="71267-181">Är mina egna databaser inställningen påverkas under skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-181">Is my custom databases setting affected during scaling?</span></span>
<span data-ttu-id="71267-182">Vissa prisnivåer har olika [databaser gränser](cache-configure.md#databases), så det finns vissa saker när skala ned om du konfigurerat ett anpassat värde för den `databases` anger under Skapa cache.</span><span class="sxs-lookup"><span data-stu-id="71267-182">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when scaling down if you configured a custom value for the `databases` setting during cache creation.</span></span>

* <span data-ttu-id="71267-183">Vid skalning till en prisnivå med en lägre `databases` gränsen än den aktuella nivån:</span><span class="sxs-lookup"><span data-stu-id="71267-183">When scaling to a pricing tier with a lower `databases` limit than the current tier:</span></span>
  * <span data-ttu-id="71267-184">Om du använder standardantalet `databases` som är 16 för alla prisnivåer inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="71267-184">If you are using the default number of `databases` which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="71267-185">Om du använder ett anpassat antal `databases` som ligger inom gränserna för den nivå som du skalning, detta `databases` inställningen bevaras och inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="71267-185">If you are using a custom number of `databases` that falls within the limits for the tier to which you are scaling, this `databases` setting is retained and no data is lost.</span></span>
  * <span data-ttu-id="71267-186">Om du använder ett anpassat antal `databases` som överskrider gränserna för ny nivå av `databases` inställningen sänks till gränser för ny nivå och alla data i de borttagna databaserna går förlorade.</span><span class="sxs-lookup"><span data-stu-id="71267-186">If you are using a custom number of `databases` that exceeds the limits of the new tier, the `databases` setting is lowered to the limits of the new tier and all data in the removed databases is lost.</span></span>
* <span data-ttu-id="71267-187">Vid skalning till en prisnivå med samma eller högre `databases` gränsen än den aktuella nivån din `databases` inställningen bevaras och inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="71267-187">When scaling to a pricing tier with the same or higher `databases` limit than the current tier your `databases` setting is retained and no data is lost.</span></span>

<span data-ttu-id="71267-188">Observera att Standard och Premium har ett SLA för 99,9% för tillgänglighet, men det finns inga SLA för förlust av data.</span><span class="sxs-lookup"><span data-stu-id="71267-188">Note that while Standard and Premium caches have a 99.9% SLA for availability, there is no SLA for data loss.</span></span>

### <a name="will-my-cache-be-available-during-scaling"></a><span data-ttu-id="71267-189">Kommer min cache vara tillgängliga under skalning?</span><span class="sxs-lookup"><span data-stu-id="71267-189">Will my cache be available during scaling?</span></span>
* <span data-ttu-id="71267-190">**Standard** och **Premium** cacheminnen finns kvar under åtgärden skalning.</span><span class="sxs-lookup"><span data-stu-id="71267-190">**Standard** and **Premium** caches remain available during the scaling operation.</span></span>
* <span data-ttu-id="71267-191">**Grundläggande** cacheminnen är offline under skalning åtgärder till en annan storlek men fortfarande är tillgängliga vid skalning från **grundläggande** till **Standard**.</span><span class="sxs-lookup"><span data-stu-id="71267-191">**Basic** caches are offline during scaling operations to a different size, but remain available when scaling from **Basic** to **Standard**.</span></span>

### <a name="operations-that-are-not-supported"></a><span data-ttu-id="71267-192">Åtgärder som inte stöds</span><span class="sxs-lookup"><span data-stu-id="71267-192">Operations that are not supported</span></span>
* <span data-ttu-id="71267-193">Du kan skala från en högre prisnivå till en lägre prisnivå.</span><span class="sxs-lookup"><span data-stu-id="71267-193">You can't scale from a higher pricing tier to a lower pricing tier.</span></span>
  * <span data-ttu-id="71267-194">Du kan skala från en **Premium** cachelagra ned till en **Standard** eller en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-194">You can't scale from a **Premium** cache down to a **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="71267-195">Du kan skala från en **Standard** cachelagra ned till en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-195">You can't scale from a **Standard** cache down to a **Basic** cache.</span></span>
* <span data-ttu-id="71267-196">Du kan skala från en **grundläggande** cachelagra till en **Standard** cache men du kan inte ändra storlek på samma gång.</span><span class="sxs-lookup"><span data-stu-id="71267-196">You can scale from a **Basic** cache to a **Standard** cache but you can't change the size at the same time.</span></span> <span data-ttu-id="71267-197">Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärd till önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="71267-197">If you need a different size, you can do a subsequent scaling operation to the desired size.</span></span>
* <span data-ttu-id="71267-198">Du kan skala från en **grundläggande** cachelagra direkt till en **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="71267-198">You can't scale from a **Basic** cache directly to a **Premium** cache.</span></span> <span data-ttu-id="71267-199">Du måste först skala från **grundläggande** till **Standard** i en skalning åtgärd och sedan från **Standard** till **Premium** vid en senare skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="71267-199">You must first scale from **Basic** to **Standard** in one scaling operation, and then from **Standard** to **Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="71267-200">Du kan skala från en större storlek för den **C0 (250 MB)** storlek.</span><span class="sxs-lookup"><span data-stu-id="71267-200">You can't scale from a larger size down to the **C0 (250 MB)** size.</span></span>

<span data-ttu-id="71267-201">Om en skalning misslyckas tjänsten kommer att försöka återställa igen och cachen återgår till den ursprungliga storleken.</span><span class="sxs-lookup"><span data-stu-id="71267-201">If a scaling operation fails, the service will try to revert the operation and the cache will revert to the original size.</span></span>

### <a name="how-long-does-scaling-take"></a><span data-ttu-id="71267-202">Hur lång tid skalning tar?</span><span class="sxs-lookup"><span data-stu-id="71267-202">How long does scaling take?</span></span>
<span data-ttu-id="71267-203">Skalning tar cirka 20 minuter beroende på hur mycket data finns i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="71267-203">Scaling takes approximately 20 minutes, depending on how much data is in the cache.</span></span>

### <a name="how-can-i-tell-when-scaling-is-complete"></a><span data-ttu-id="71267-204">Hur vet jag när skalning är klar?</span><span class="sxs-lookup"><span data-stu-id="71267-204">How can I tell when scaling is complete?</span></span>
<span data-ttu-id="71267-205">Du kan se skalning pågår i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="71267-205">In the Azure portal you can see the scaling operation in progress.</span></span> <span data-ttu-id="71267-206">När skalning är klar, status för cachen ändras till **kör**.</span><span class="sxs-lookup"><span data-stu-id="71267-206">When scaling is complete, the status of the cache changes to **Running**.</span></span>

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



