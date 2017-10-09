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
# <a name="how-tooscale-azure-redis-cache"></a><span data-ttu-id="8bd2a-103">Hur tooScale Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="8bd2a-103">How tooScale Azure Redis Cache</span></span>
<span data-ttu-id="8bd2a-104">Azure Redis-Cache har olika cache erbjudanden, vilket ger flexibilitet i hello valet av cachestorlek och funktioner.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-104">Azure Redis Cache has different cache offerings, which provide flexibility in hello choice of cache size and features.</span></span> <span data-ttu-id="8bd2a-105">När en cache har skapats kan skala du hello storlek och hello prisnivån hello cache ändrar hello kraven i ditt program.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-105">After a cache is created, you can scale hello size and hello pricing tier of hello cache if hello requirements of your application change.</span></span> <span data-ttu-id="8bd2a-106">Den här artikeln visar hur tooscale ditt cacheminne i hello Azure-portalen och använda verktyg som Azure PowerShell och Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-106">This article shows you how tooscale your cache in both hello Azure portal and using tools such as Azure PowerShell and Azure CLI.</span></span>

## <a name="when-tooscale"></a><span data-ttu-id="8bd2a-107">När tooscale</span><span class="sxs-lookup"><span data-stu-id="8bd2a-107">When tooscale</span></span>
<span data-ttu-id="8bd2a-108">Du kan använda hello [övervakning](cache-how-to-monitor.md) funktioner i Azure Redis-Cache toomonitor hello hälsotillstånd och prestanda för ditt cacheminne och hjälper dem att avgöra när tooscale hello cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-108">You can use hello [monitoring](cache-how-to-monitor.md) features of Azure Redis Cache toomonitor hello health and performance of your cache and help determine when tooscale hello cache.</span></span> 

<span data-ttu-id="8bd2a-109">Du kan övervaka hello följande mått toohelp Bestäm om du behöver tooscale.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-109">You can monitor hello following metrics toohelp determine if you need tooscale.</span></span>

* <span data-ttu-id="8bd2a-110">Redis-serverbelastning</span><span class="sxs-lookup"><span data-stu-id="8bd2a-110">Redis Server Load</span></span>
* <span data-ttu-id="8bd2a-111">Minnesanvändning</span><span class="sxs-lookup"><span data-stu-id="8bd2a-111">Memory Usage</span></span>
* <span data-ttu-id="8bd2a-112">Nätverkets bandbredd</span><span class="sxs-lookup"><span data-stu-id="8bd2a-112">Network Bandwidth</span></span>
* <span data-ttu-id="8bd2a-113">CPU-användning</span><span class="sxs-lookup"><span data-stu-id="8bd2a-113">CPU Usage</span></span>

<span data-ttu-id="8bd2a-114">Om du finner att ditt cacheminne är inte längre uppfyller kraven för ditt program, kan du skala tooa större eller mindre cache prisnivån som passar ditt program.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-114">If you determine that your cache is no longer meeting your application's requirements, you can scale tooa larger or smaller cache pricing tier that is right for your application.</span></span> <span data-ttu-id="8bd2a-115">Läs mer om hur du avgör vilket cachelagra prisnivå nivå toouse [vilka Redis-Cache erbjudande och vilken storlek ska jag använda](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span><span class="sxs-lookup"><span data-stu-id="8bd2a-115">For more information on determining which cache pricing tier toouse, see [What Redis Cache offering and size should I use](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).</span></span>

## <a name="scale-a-cache"></a><span data-ttu-id="8bd2a-116">Skala en cache</span><span class="sxs-lookup"><span data-stu-id="8bd2a-116">Scale a cache</span></span>
<span data-ttu-id="8bd2a-117">tooscale ditt cacheminne [Bläddra toohello cache](cache-configure.md#configure-redis-cache-settings) i hello [Azure-portalen](https://portal.azure.com) och på **skala** från hello **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-117">tooscale your cache, [browse toohello cache](cache-configure.md#configure-redis-cache-settings) in hello [Azure portal](https://portal.azure.com) and click **Scale** from hello **Resource menu**.</span></span>

![Skala](./media/cache-how-to-scale/redis-cache-scale-menu.png)

<span data-ttu-id="8bd2a-119">Välj hello önskade prisnivån från hello **Välj prisnivån** bladet och klicka på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-119">Select hello desired pricing tier from hello **Select pricing tier** blade and click **Select**.</span></span>

![Prisnivå][redis-cache-pricing-tier-blade]


<span data-ttu-id="8bd2a-121">Du kan skala tooa olika prisnivån med hello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="8bd2a-121">You can scale tooa different pricing tier with hello following restrictions:</span></span>

* <span data-ttu-id="8bd2a-122">Du kan skala från en högre prisnivå nivå tooa lägre prisnivån.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-122">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
  * <span data-ttu-id="8bd2a-123">Du kan skala från en **Premium** cache ned tooa **Standard** eller en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-123">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="8bd2a-124">Du kan skala från en **Standard** cache ned tooa **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-124">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
* <span data-ttu-id="8bd2a-125">Du kan skala från en **grundläggande** cachelagra tooa **Standard** cache men du kan inte ändra hello storleken på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-125">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="8bd2a-126">Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärden toohello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-126">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
* <span data-ttu-id="8bd2a-127">Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-127">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="8bd2a-128">Du måste skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** i ett efterföljande skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-128">You must scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="8bd2a-129">Du kan skala från en större storlek ned toohello **C0 (250 MB)** storlek.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-129">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>
 
<span data-ttu-id="8bd2a-130">Medan hello cache skalas toohello nya prisnivån, en **skalning** status visas i hello **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-130">While hello cache is scaling toohello new pricing tier, a **Scaling** status is displayed in hello **Redis Cache** blade.</span></span>

![Skalning][redis-cache-scaling]

<span data-ttu-id="8bd2a-132">När skalning är klar hello status ändras från **skalning** för**kör**.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-132">When scaling is complete, hello status changes from **Scaling** too**Running**.</span></span>

## <a name="how-tooautomate-a-scaling-operation"></a><span data-ttu-id="8bd2a-133">Hur tooautomate skalning åtgärden</span><span class="sxs-lookup"><span data-stu-id="8bd2a-133">How tooautomate a scaling operation</span></span>
<span data-ttu-id="8bd2a-134">I tillägg tooscaling hello cacheinstanser i Azure-portalen kan du skala med PowerShell-cmdletar, Azure CLI och hello med hjälp av Microsoft Azure Management bibliotek (MAML).</span><span class="sxs-lookup"><span data-stu-id="8bd2a-134">In addition tooscaling your cache instances in hello Azure portal, you can scale using PowerShell cmdlets, Azure CLI, and by using hello Microsoft Azure Management Libraries (MAML).</span></span> 

* [<span data-ttu-id="8bd2a-135">Skala med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd2a-135">Scale using PowerShell</span></span>](#scale-using-powershell)
* [<span data-ttu-id="8bd2a-136">Skala med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8bd2a-136">Scale using Azure CLI</span></span>](#scale-using-azure-cli)
* [<span data-ttu-id="8bd2a-137">Skala med MAML</span><span class="sxs-lookup"><span data-stu-id="8bd2a-137">Scale using MAML</span></span>](#scale-using-maml)

### <a name="scale-using-powershell"></a><span data-ttu-id="8bd2a-138">Skala med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="8bd2a-138">Scale using PowerShell</span></span>
<span data-ttu-id="8bd2a-139">Du kan skala dina Azure Redis-Cache-instanser med PowerShell med hjälp av hello [Set AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet när hello `Size`, `Sku`, eller `ShardCount` ändras egenskaperna.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-139">You can scale your Azure Redis Cache instances with PowerShell by using hello [Set-AzureRmRedisCache](https://msdn.microsoft.com/library/azure/mt634518.aspx) cmdlet when hello `Size`, `Sku`, or `ShardCount` properties are modified.</span></span> <span data-ttu-id="8bd2a-140">hello följande exempel visas hur tooscale en cache med namnet `myCache` tooa 2,5 GB cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-140">hello following example shows how tooscale a cache named `myCache` tooa 2.5 GB cache.</span></span> 

    Set-AzureRmRedisCache -ResourceGroupName myGroup -Name myCache -Size 2.5GB

<span data-ttu-id="8bd2a-141">Läs mer om att skala med PowerShell [tooscale ett Redis-cache med hjälp av Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span><span class="sxs-lookup"><span data-stu-id="8bd2a-141">For more information on scaling with PowerShell, see [tooscale a Redis cache using Powershell](cache-howto-manage-redis-cache-powershell.md#scale).</span></span>

### <a name="scale-using-azure-cli"></a><span data-ttu-id="8bd2a-142">Skala med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="8bd2a-142">Scale using Azure CLI</span></span>
<span data-ttu-id="8bd2a-143">tooscale din Azure Redis-Cache-instanser som använder Azure CLI, anropa hello `azure rediscache set` kommando- och pass i hello önskad konfigurationsändringar som innehåller en ny storlek eller sku klusterstorleken, beroende på hello önskad skalning igen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-143">tooscale your Azure Redis Cache instances using Azure CLI, call hello `azure rediscache set` command and pass in hello desired configuration changes that include a new size, sku, or cluster size, depending on hello desired scaling operation.</span></span>

<span data-ttu-id="8bd2a-144">Mer information om att skala med Azure CLI finns [ändra inställningar för en befintlig Redis-Cache](cache-manage-cli.md#scale).</span><span class="sxs-lookup"><span data-stu-id="8bd2a-144">For more information on scaling with Azure CLI, see [Change settings of an existing Redis Cache](cache-manage-cli.md#scale).</span></span>

### <a name="scale-using-maml"></a><span data-ttu-id="8bd2a-145">Skala med MAML</span><span class="sxs-lookup"><span data-stu-id="8bd2a-145">Scale using MAML</span></span>
<span data-ttu-id="8bd2a-146">tooscale din Azure Redis-Cache-instanser som använder hello [Microsoft Azure Management bibliotek (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), anrop hello `IRedisOperations.CreateOrUpdate` metod och skicka hello nya storleken för hello `RedisProperties.SKU.Capacity`.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-146">tooscale your Azure Redis Cache instances using hello [Microsoft Azure Management Libraries (MAML)](http://azure.microsoft.com/updates/management-libraries-for-net-release-announcement/), call hello `IRedisOperations.CreateOrUpdate` method and pass in hello new size for hello `RedisProperties.SKU.Capacity`.</span></span>

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

<span data-ttu-id="8bd2a-147">Mer information finns i hello [hantera Redis-Cache med MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) exempel.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-147">For more information, see hello [Manage Redis Cache using MAML](https://github.com/rustd/RedisSamples/tree/master/ManageCacheUsingMAML) sample.</span></span>

## <a name="scaling-faq"></a><span data-ttu-id="8bd2a-148">Skalning vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="8bd2a-148">Scaling FAQ</span></span>
<span data-ttu-id="8bd2a-149">hello innehåller följande lista svar toocommonly frågor och svar om Azure Redis-Cache skalning.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-149">hello following list contains answers toocommonly asked questions about Azure Redis Cache scaling.</span></span>

* [<span data-ttu-id="8bd2a-150">Kan jag skala till, från eller inom en Premium-cache?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-150">Can I scale to, from, or within a Premium cache?</span></span>](#can-i-scale-to-from-or-within-a-premium-cache)
* [<span data-ttu-id="8bd2a-151">Efter skalning finns det toochange Mina cache namn eller åtkomst nycklar?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-151">After scaling, do I have toochange my cache name or access keys?</span></span>](#after-scaling-do-i-have-to-change-my-cache-name-or-access-keys)
* [<span data-ttu-id="8bd2a-152">Hur fungerar skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-152">How does scaling work?</span></span>](#how-does-scaling-work)
* [<span data-ttu-id="8bd2a-153">Jag förlorar data från min cache under skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-153">Will I lose data from my cache during scaling?</span></span>](#will-i-lose-data-from-my-cache-during-scaling)
* [<span data-ttu-id="8bd2a-154">Är mina egna databaser inställningen påverkas under skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-154">Is my custom databases setting affected during scaling?</span></span>](#is-my-custom-databases-setting-affected-during-scaling)
* [<span data-ttu-id="8bd2a-155">Kommer min cache vara tillgängliga under skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-155">Will my cache be available during scaling?</span></span>](#will-my-cache-be-available-during-scaling)
* [<span data-ttu-id="8bd2a-156">Åtgärder som inte stöds</span><span class="sxs-lookup"><span data-stu-id="8bd2a-156">Operations that are not supported</span></span>](#operations-that-are-not-supported)
* [<span data-ttu-id="8bd2a-157">Hur lång tid skalning tar?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-157">How long does scaling take?</span></span>](#how-long-does-scaling-take)
* [<span data-ttu-id="8bd2a-158">Hur vet jag när skalning är klar?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-158">How can I tell when scaling is complete?</span></span>](#how-can-i-tell-when-scaling-is-complete)

### <a name="can-i-scale-to-from-or-within-a-premium-cache"></a><span data-ttu-id="8bd2a-159">Kan jag skala till, från eller inom en Premium-cache?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-159">Can I scale to, from, or within a Premium cache?</span></span>
* <span data-ttu-id="8bd2a-160">Du kan skala från en **Premium** cache ned tooa **grundläggande** eller **Standard** prisnivån.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-160">You can't scale from a **Premium** cache down tooa **Basic** or **Standard** pricing tier.</span></span>
* <span data-ttu-id="8bd2a-161">Du kan skala från en **Premium** cache priser nivå tooanother.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-161">You can scale from one **Premium** cache pricing tier tooanother.</span></span>
* <span data-ttu-id="8bd2a-162">Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-162">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="8bd2a-163">Du måste först skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** vid en senare skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-163">You must first scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="8bd2a-164">Om du har aktiverat kluster när du skapade din **Premium** cache, kan du [ändra hello klusterstorleken](cache-how-to-premium-clustering.md#cluster-size).</span><span class="sxs-lookup"><span data-stu-id="8bd2a-164">If you enabled clustering when you created your **Premium** cache, you can [change hello cluster size](cache-how-to-premium-clustering.md#cluster-size).</span></span> <span data-ttu-id="8bd2a-165">Om ditt cacheminne har skapats utan kluster aktiverad, kan du inte konfigurera klustring vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-165">If your cache was created without clustering enabled, you can't configure clustering at a later time.</span></span>
  
  <span data-ttu-id="8bd2a-166">Mer information finns i [hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="8bd2a-166">For more information, see [How tooconfigure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>

### <a name="after-scaling-do-i-have-toochange-my-cache-name-or-access-keys"></a><span data-ttu-id="8bd2a-167">Efter skalning finns det toochange Mina cache namn eller åtkomst nycklar?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-167">After scaling, do I have toochange my cache name or access keys?</span></span>
<span data-ttu-id="8bd2a-168">Nej, cache namn och nycklar är oförändrade under en åtgärd med skalning.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-168">No, your cache name and keys are unchanged during a scaling operation.</span></span>

### <a name="how-does-scaling-work"></a><span data-ttu-id="8bd2a-169">Hur fungerar skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-169">How does scaling work?</span></span>
* <span data-ttu-id="8bd2a-170">När en **grundläggande** cache skalas tooa olika storlek, den är avstängd och ett nytt cacheminne har etablerats med hello nya storleken.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-170">When a **Basic** cache is scaled tooa different size, it is shut down and a new cache is provisioned using hello new size.</span></span> <span data-ttu-id="8bd2a-171">Under denna tid hello cache är inte tillgänglig och alla data i hello cacheminnet har gått förlorade.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-171">During this time, hello cache is unavailable and all data in hello cache is lost.</span></span>
* <span data-ttu-id="8bd2a-172">När en **grundläggande** cache är skalade tooa **Standard** cache, en replik-cache är etablerad och hello data kopieras från hello primära cache toohello replik cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-172">When a **Basic** cache is scaled tooa **Standard** cache, a replica cache is provisioned and hello data is copied from hello primary cache toohello replica cache.</span></span> <span data-ttu-id="8bd2a-173">hello cache finns kvar under hello skalning processen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-173">hello cache remains available during hello scaling process.</span></span>
* <span data-ttu-id="8bd2a-174">När en **Standard** cache är skalade tooa annan storlek eller tooa **Premium** cache, en hello repliker är avstängd och nytt etableras toohello nya storlek och hello data överförs och hello andra replik utför en växling vid fel innan etableras igen, liknande toohello process som sker vid fel på en hello cache-noder.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-174">When a **Standard** cache is scaled tooa different size or tooa **Premium** cache, one of hello replicas is shut down and re-provisioned toohello new size and hello data transferred over, and then hello other replica performs a failover before it is re-provisioned, similar toohello process that occurs during a failure of one of hello cache nodes.</span></span>

### <a name="will-i-lose-data-from-my-cache-during-scaling"></a><span data-ttu-id="8bd2a-175">Jag förlorar data från min cache under skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-175">Will I lose data from my cache during scaling?</span></span>
* <span data-ttu-id="8bd2a-176">När en **grundläggande** cache är skalade tooa nya storlek, alla data går förlorad och hello cachen inte är tillgänglig under hello skalning igen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-176">When a **Basic** cache is scaled tooa new size, all data is lost and hello cache is unavailable during hello scaling operation.</span></span>
* <span data-ttu-id="8bd2a-177">När en **grundläggande** cache är skalade tooa **Standard** cache, hello data i cacheminnet hello vanligtvis bevaras.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-177">When a **Basic** cache is scaled tooa **Standard** cache, hello data in hello cache is typically preserved.</span></span>
* <span data-ttu-id="8bd2a-178">När en **Standard** cache är skalade tooa större storlek eller nivån eller en **Premium** cache skalas tooa större storlek, alla data bevaras normalt.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-178">When a **Standard** cache is scaled tooa larger size or tier, or a **Premium** cache is scaled tooa larger size, all data is typically preserved.</span></span> <span data-ttu-id="8bd2a-179">Vid skalning en **Standard** eller **Premium** cache ned tooa mindre storlek, data kan gå förlorade beroende på hur mycket data finns i cacheminnet för hello relaterade toohello nya storleken när skalas.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-179">When scaling a **Standard** or **Premium** cache down tooa smaller size, data may be lost depending on how much data is in hello cache related toohello new size when it is scaled.</span></span> <span data-ttu-id="8bd2a-180">Om data förloras vid skalning nycklar avlägsnas med hello [allkeys lru](http://redis.io/topics/lru-cache) av principen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-180">If data is lost when scaling down, keys are evicted using hello [allkeys-lru](http://redis.io/topics/lru-cache) eviction policy.</span></span> 

### <a name="is-my-custom-databases-setting-affected-during-scaling"></a><span data-ttu-id="8bd2a-181">Är mina egna databaser inställningen påverkas under skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-181">Is my custom databases setting affected during scaling?</span></span>
<span data-ttu-id="8bd2a-182">Vissa prisnivåer har olika [databaser gränser](cache-configure.md#databases), så det finns vissa saker när skala ned om du konfigurerat ett anpassat värde för hello `databases` anger under Skapa cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-182">Some pricing tiers have different [databases limits](cache-configure.md#databases), so there are some considerations when scaling down if you configured a custom value for hello `databases` setting during cache creation.</span></span>

* <span data-ttu-id="8bd2a-183">När skalning tooa prisnivån med en lägre `databases` gränsen än hello aktuella nivån:</span><span class="sxs-lookup"><span data-stu-id="8bd2a-183">When scaling tooa pricing tier with a lower `databases` limit than hello current tier:</span></span>
  * <span data-ttu-id="8bd2a-184">Om du använder hello standardantalet `databases` som är 16 för alla prisnivåer inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-184">If you are using hello default number of `databases` which is 16 for all pricing tiers, no data is lost.</span></span>
  * <span data-ttu-id="8bd2a-185">Om du använder ett anpassat antal `databases` som faller inom hello gränser för hello nivå toowhich du skalning, detta `databases` inställningen bevaras och inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-185">If you are using a custom number of `databases` that falls within hello limits for hello tier toowhich you are scaling, this `databases` setting is retained and no data is lost.</span></span>
  * <span data-ttu-id="8bd2a-186">Om du använder ett anpassat antal `databases` som överskrider hello gränserna för hello ny nivå, hello `databases` inställningen är nedsänkt toohello gränserna för hello ny nivå och alla data i hello bort databaser har gått förlorade.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-186">If you are using a custom number of `databases` that exceeds hello limits of hello new tier, hello `databases` setting is lowered toohello limits of hello new tier and all data in hello removed databases is lost.</span></span>
* <span data-ttu-id="8bd2a-187">När skalning tooa prisnivån med hello samma eller högre `databases` gränsen än hello aktuella nivån din `databases` inställningen bevaras och inga data går förlorade.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-187">When scaling tooa pricing tier with hello same or higher `databases` limit than hello current tier your `databases` setting is retained and no data is lost.</span></span>

<span data-ttu-id="8bd2a-188">Observera att Standard och Premium har ett SLA för 99,9% för tillgänglighet, men det finns inga SLA för förlust av data.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-188">Note that while Standard and Premium caches have a 99.9% SLA for availability, there is no SLA for data loss.</span></span>

### <a name="will-my-cache-be-available-during-scaling"></a><span data-ttu-id="8bd2a-189">Kommer min cache vara tillgängliga under skalning?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-189">Will my cache be available during scaling?</span></span>
* <span data-ttu-id="8bd2a-190">**Standard** och **Premium** cacheminnen vara tillgängliga under hello skalning igen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-190">**Standard** and **Premium** caches remain available during hello scaling operation.</span></span>
* <span data-ttu-id="8bd2a-191">**Grundläggande** cacheminnen offline under skalning operations tooa olika storlek, men fortfarande är tillgängliga vid skalning från **grundläggande** för**Standard**.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-191">**Basic** caches are offline during scaling operations tooa different size, but remain available when scaling from **Basic** too**Standard**.</span></span>

### <a name="operations-that-are-not-supported"></a><span data-ttu-id="8bd2a-192">Åtgärder som inte stöds</span><span class="sxs-lookup"><span data-stu-id="8bd2a-192">Operations that are not supported</span></span>
* <span data-ttu-id="8bd2a-193">Du kan skala från en högre prisnivå nivå tooa lägre prisnivån.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-193">You can't scale from a higher pricing tier tooa lower pricing tier.</span></span>
  * <span data-ttu-id="8bd2a-194">Du kan skala från en **Premium** cache ned tooa **Standard** eller en **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-194">You can't scale from a **Premium** cache down tooa **Standard** or a **Basic** cache.</span></span>
  * <span data-ttu-id="8bd2a-195">Du kan skala från en **Standard** cache ned tooa **grundläggande** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-195">You can't scale from a **Standard** cache down tooa **Basic** cache.</span></span>
* <span data-ttu-id="8bd2a-196">Du kan skala från en **grundläggande** cachelagra tooa **Standard** cache men du kan inte ändra hello storleken på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-196">You can scale from a **Basic** cache tooa **Standard** cache but you can't change hello size at hello same time.</span></span> <span data-ttu-id="8bd2a-197">Om du behöver olika storlek, kan du göra en efterföljande skalning åtgärden toohello önskad storlek.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-197">If you need a different size, you can do a subsequent scaling operation toohello desired size.</span></span>
* <span data-ttu-id="8bd2a-198">Du kan skala från en **grundläggande** cachelagra direkt tooa **Premium** cache.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-198">You can't scale from a **Basic** cache directly tooa **Premium** cache.</span></span> <span data-ttu-id="8bd2a-199">Du måste först skala från **grundläggande** för**Standard** i en skalning åtgärd och sedan från **Standard** för**Premium** vid en senare skalning åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-199">You must first scale from **Basic** too**Standard** in one scaling operation, and then from **Standard** too**Premium** in a subsequent scaling operation.</span></span>
* <span data-ttu-id="8bd2a-200">Du kan skala från en större storlek ned toohello **C0 (250 MB)** storlek.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-200">You can't scale from a larger size down toohello **C0 (250 MB)** size.</span></span>

<span data-ttu-id="8bd2a-201">Om en skalning misslyckas hello tjänsten kommer försök toorevert hello och hello cache återställs toohello ursprungliga storleken.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-201">If a scaling operation fails, hello service will try toorevert hello operation and hello cache will revert toohello original size.</span></span>

### <a name="how-long-does-scaling-take"></a><span data-ttu-id="8bd2a-202">Hur lång tid skalning tar?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-202">How long does scaling take?</span></span>
<span data-ttu-id="8bd2a-203">Skalning tar cirka 20 minuter beroende på hur mycket data är i hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-203">Scaling takes approximately 20 minutes, depending on how much data is in hello cache.</span></span>

### <a name="how-can-i-tell-when-scaling-is-complete"></a><span data-ttu-id="8bd2a-204">Hur vet jag när skalning är klar?</span><span class="sxs-lookup"><span data-stu-id="8bd2a-204">How can I tell when scaling is complete?</span></span>
<span data-ttu-id="8bd2a-205">Du kan se hello skalning åtgärden pågår i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-205">In hello Azure portal you can see hello scaling operation in progress.</span></span> <span data-ttu-id="8bd2a-206">När skalning är klar hello status hello cache ändras för**kör**.</span><span class="sxs-lookup"><span data-stu-id="8bd2a-206">When scaling is complete, hello status of hello cache changes too**Running**.</span></span>

<!-- IMAGES -->

[redis-cache-pricing-tier-blade]: ./media/cache-how-to-scale/redis-cache-pricing-tier-blade.png

[redis-cache-scaling]: ./media/cache-how-to-scale/redis-cache-scaling.png



