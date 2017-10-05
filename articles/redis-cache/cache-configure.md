---
title: "Så här konfigurerar du Azure Redis-Cache | Microsoft Docs"
description: "Förstå Redis standardkonfigurationen för Azure Redis-Cache och lära dig hur du konfigurerar Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: na
author: steved0x
manager: douge
editor: tysonn
ms.assetid: d0bf2e1f-6a26-4e62-85ba-d82b35fc5aa6
ms.service: cache
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: cache-redis
ms.workload: tbd
ms.date: 08/22/2017
ms.author: sdanie
ms.openlocfilehash: 0274e58eb2e83202d4dbc58da0c67d0fdde22ede
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-azure-redis-cache"></a><span data-ttu-id="fb0f4-103">Så här konfigurerar du Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="fb0f4-103">How to configure Azure Redis Cache</span></span>
<span data-ttu-id="fb0f4-104">Det här avsnittet beskriver hur du granska och uppdatera konfigurationen för Azure Redis-Cache-instanser och täcker standardkonfigurationen för Redis-servern för Azure Redis-Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-104">This topic describes how to review and update the configuration for your Azure Redis Cache instances, and covers the default Redis server configuration for Azure Redis Cache instances.</span></span>

> [!NOTE]
> <span data-ttu-id="fb0f4-105">Mer information om hur du konfigurerar och använder premium cache-funktioner finns [hur du konfigurerar persistence](cache-how-to-premium-persistence.md), [konfigurera klustring](cache-how-to-premium-clustering.md), och [så här konfigurerar du stöd för virtuella nätverk ](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-105">For more information on configuring and using premium cache features, see [How to configure persistence](cache-how-to-premium-persistence.md), [How to configure clustering](cache-how-to-premium-clustering.md), and [How to configure Virtual Network support](cache-how-to-premium-vnet.md).</span></span>
> 
> 

## <a name="configure-redis-cache-settings"></a><span data-ttu-id="fb0f4-106">Konfigurera inställningar för Redis-cache</span><span class="sxs-lookup"><span data-stu-id="fb0f4-106">Configure Redis cache settings</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="fb0f4-107">Azure Redis-Cache-inställningar visas och konfigureras på den **Redis-Cache** bladet med hjälp av den **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-107">Azure Redis Cache settings are viewed and configured on the **Redis Cache** blade using the **Resource Menu**.</span></span>

![Redis-Cache-inställningar](./media/cache-configure/redis-cache-settings.png)

<span data-ttu-id="fb0f4-109">Du kan visa och konfigurera följande inställningar med hjälp av den **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-109">You can view and configure the following settings using the **Resource Menu**.</span></span>

* [<span data-ttu-id="fb0f4-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="fb0f4-110">Overview</span></span>](#overview)
* [<span data-ttu-id="fb0f4-111">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="fb0f4-111">Activity log</span></span>](#activity-log)
* [<span data-ttu-id="fb0f4-112">Åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-112">Access control (IAM)</span></span>](#access-control-iam)
* [<span data-ttu-id="fb0f4-113">Taggar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-113">Tags</span></span>](#tags)
* [<span data-ttu-id="fb0f4-114">Diagnostisera och lösa problem</span><span class="sxs-lookup"><span data-stu-id="fb0f4-114">Diagnose and solve problems</span></span>](#diagnose-and-solve-problems)
* [<span data-ttu-id="fb0f4-115">Inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-115">Settings</span></span>](#settings)
    * [<span data-ttu-id="fb0f4-116">Snabbtangenter</span><span class="sxs-lookup"><span data-stu-id="fb0f4-116">Access keys</span></span>](#access-keys)
    * [<span data-ttu-id="fb0f4-117">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-117">Advanced settings</span></span>](#advanced-settings)
    * [<span data-ttu-id="fb0f4-118">Redis-Cache Advisor</span><span class="sxs-lookup"><span data-stu-id="fb0f4-118">Redis Cache Advisor</span></span>](#redis-cache-advisor)
    * [<span data-ttu-id="fb0f4-119">Skalning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-119">Scale</span></span>](#scale)
    * [<span data-ttu-id="fb0f4-120">Redis klusterstorleken</span><span class="sxs-lookup"><span data-stu-id="fb0f4-120">Redis cluster size</span></span>](#cluster-size)
    * [<span data-ttu-id="fb0f4-121">Redis-datapersistence</span><span class="sxs-lookup"><span data-stu-id="fb0f4-121">Redis data persistence</span></span>](#redis-data-persistence)
    * [<span data-ttu-id="fb0f4-122">Schemauppdateringar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-122">Schedule updates</span></span>](#schedule-updates)
    * [<span data-ttu-id="fb0f4-123">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="fb0f4-123">Geo-replication</span></span>](#geo-replication)
    * [<span data-ttu-id="fb0f4-124">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="fb0f4-124">Virtual Network</span></span>](#virtual-network)
    * [<span data-ttu-id="fb0f4-125">Brandvägg</span><span class="sxs-lookup"><span data-stu-id="fb0f4-125">Firewall</span></span>](#firewall)
    * [<span data-ttu-id="fb0f4-126">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="fb0f4-126">Properties</span></span>](#properties)
    * [<span data-ttu-id="fb0f4-127">Lås</span><span class="sxs-lookup"><span data-stu-id="fb0f4-127">Locks</span></span>](#locks)
    * [<span data-ttu-id="fb0f4-128">Automation-skript</span><span class="sxs-lookup"><span data-stu-id="fb0f4-128">Automation script</span></span>](#automation-script)
* [<span data-ttu-id="fb0f4-129">Administration</span><span class="sxs-lookup"><span data-stu-id="fb0f4-129">Administration</span></span>](#administration)
    * [<span data-ttu-id="fb0f4-130">Importera data</span><span class="sxs-lookup"><span data-stu-id="fb0f4-130">Import data</span></span>](#importexport)
    * [<span data-ttu-id="fb0f4-131">Exportera data</span><span class="sxs-lookup"><span data-stu-id="fb0f4-131">Export data</span></span>](#importexport)
    * [<span data-ttu-id="fb0f4-132">Starta om</span><span class="sxs-lookup"><span data-stu-id="fb0f4-132">Reboot</span></span>](#reboot)
* [<span data-ttu-id="fb0f4-133">Övervakning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-133">Monitoring</span></span>](#monitoring)
    * [<span data-ttu-id="fb0f4-134">Redis-mått</span><span class="sxs-lookup"><span data-stu-id="fb0f4-134">Redis metrics</span></span>](#redis-metrics)
    * [<span data-ttu-id="fb0f4-135">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="fb0f4-135">Alert rules</span></span>](#alert-rules)
    * [<span data-ttu-id="fb0f4-136">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="fb0f4-136">Diagnostics</span></span>](#diagnostics)
* [<span data-ttu-id="fb0f4-137">Support och felsökning av inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-137">Support & troubleshooting settings</span></span>](#support-amp-troubleshooting-settings)
    * [<span data-ttu-id="fb0f4-138">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="fb0f4-138">Resource health</span></span>](#resource-health)
    * [<span data-ttu-id="fb0f4-139">Ny supportbegäran</span><span class="sxs-lookup"><span data-stu-id="fb0f4-139">New support request</span></span>](#new-support-request)


## <a name="overview"></a><span data-ttu-id="fb0f4-140">Översikt</span><span class="sxs-lookup"><span data-stu-id="fb0f4-140">Overview</span></span>

<span data-ttu-id="fb0f4-141">**Översikt över** ger dig med grundläggande information om ditt cacheminne, till exempel namn, portar, prisnivå, och valda cache mått.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-141">**Overview** provides you with basic information about your cache, such as name, ports, pricing tier, and selected cache metrics.</span></span>

### <a name="activity-log"></a><span data-ttu-id="fb0f4-142">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="fb0f4-142">Activity log</span></span>

<span data-ttu-id="fb0f4-143">Klicka på **aktivitetsloggen** att visa åtgärder som utförs på ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-143">Click **Activity log** to view actions performed on your cache.</span></span> <span data-ttu-id="fb0f4-144">Du kan också använda filtrering för att expandera den här vyn om du vill inkludera andra resurser.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-144">You can also use filtering to expand this view to include other resources.</span></span> <span data-ttu-id="fb0f4-145">Mer information om hur du arbetar med granskningsloggarna finns [granskningsåtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-145">For more information on working with audit logs, see [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md).</span></span> <span data-ttu-id="fb0f4-146">Mer information om hur du övervakar Azure Redis-Cache-händelser finns [åtgärder och aviseringar](cache-how-to-monitor.md#operations-and-alerts).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-146">For more information on monitoring Azure Redis Cache events, see [Operations and alerts](cache-how-to-monitor.md#operations-and-alerts).</span></span>

### <a name="access-control-iam"></a><span data-ttu-id="fb0f4-147">Åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-147">Access control (IAM)</span></span>

<span data-ttu-id="fb0f4-148">Den **åtkomstkontroll (IAM)** avsnittet ger stöd för rollbaserad åtkomstkontroll (RBAC) i Azure portal för att hjälpa företag att uppfylla krav deras åtkomst bara och exakt.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-148">The **Access control (IAM)** section provides support for role-based access control (RBAC) in the Azure portal to help organizations meet their access management requirements simply and precisely.</span></span> <span data-ttu-id="fb0f4-149">Mer information finns i [rollbaserad åtkomstkontroll i Azure portal](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-149">For more information, see [Role-based access control in the Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>

### <a name="tags"></a><span data-ttu-id="fb0f4-150">Taggar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-150">Tags</span></span>

<span data-ttu-id="fb0f4-151">Den **taggar** avsnitt kan du ordna dina resurser.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-151">The **Tags** section helps you organize your resources.</span></span> <span data-ttu-id="fb0f4-152">Mer information finns i [med taggar för att organisera dina Azure-resurser](../azure-resource-manager/resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-152">For more information, see [Using tags to organize your Azure resources](../azure-resource-manager/resource-group-using-tags.md).</span></span>


### <a name="diagnose-and-solve-problems"></a><span data-ttu-id="fb0f4-153">Diagnosticera och lösa problem</span><span class="sxs-lookup"><span data-stu-id="fb0f4-153">Diagnose and solve problems</span></span>

<span data-ttu-id="fb0f4-154">Klicka på **diagnostisera och lösa problem** måste anges med vanliga problem och strategier för att lösa dem..</span><span class="sxs-lookup"><span data-stu-id="fb0f4-154">Click **Diagnose and solve problems** to be provided with common issues and strategies for resolving them.</span></span>



## <a name="settings"></a><span data-ttu-id="fb0f4-155">Inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-155">Settings</span></span>
<span data-ttu-id="fb0f4-156">Den **inställningar** avsnittet kan du komma åt och konfigurera följande inställningar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-156">The **Settings** section allows you to access and configure the following settings for your cache.</span></span>

* [<span data-ttu-id="fb0f4-157">Snabbtangenter</span><span class="sxs-lookup"><span data-stu-id="fb0f4-157">Access keys</span></span>](#access-keys)
* [<span data-ttu-id="fb0f4-158">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-158">Advanced settings</span></span>](#advanced-settings)
* [<span data-ttu-id="fb0f4-159">Redis-Cache Advisor</span><span class="sxs-lookup"><span data-stu-id="fb0f4-159">Redis Cache Advisor</span></span>](#redis-cache-advisor)
* [<span data-ttu-id="fb0f4-160">Skalning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-160">Scale</span></span>](#scale)
* [<span data-ttu-id="fb0f4-161">Redis klusterstorleken</span><span class="sxs-lookup"><span data-stu-id="fb0f4-161">Redis cluster size</span></span>](#cluster-size)
* [<span data-ttu-id="fb0f4-162">Redis-datapersistence</span><span class="sxs-lookup"><span data-stu-id="fb0f4-162">Redis data persistence</span></span>](#redis-data-persistence)
* [<span data-ttu-id="fb0f4-163">Schemauppdateringar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-163">Schedule updates</span></span>](#schedule-updates)
* [<span data-ttu-id="fb0f4-164">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="fb0f4-164">Geo-replication</span></span>](#geo-replication)
* [<span data-ttu-id="fb0f4-165">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="fb0f4-165">Virtual Network</span></span>](#virtual-network)
* [<span data-ttu-id="fb0f4-166">Brandvägg</span><span class="sxs-lookup"><span data-stu-id="fb0f4-166">Firewall</span></span>](#firewall)
* [<span data-ttu-id="fb0f4-167">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="fb0f4-167">Properties</span></span>](#properties)
* [<span data-ttu-id="fb0f4-168">Lås</span><span class="sxs-lookup"><span data-stu-id="fb0f4-168">Locks</span></span>](#locks)
* [<span data-ttu-id="fb0f4-169">Automation-skript</span><span class="sxs-lookup"><span data-stu-id="fb0f4-169">Automation script</span></span>](#automation-script)



### <a name="access-keys"></a><span data-ttu-id="fb0f4-170">Åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-170">Access keys</span></span>
<span data-ttu-id="fb0f4-171">Klicka på **åtkomstnycklar** att visa eller återskapa åtkomstnycklarna för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-171">Click **Access keys** to view or regenerate the access keys for your cache.</span></span> <span data-ttu-id="fb0f4-172">De här nycklarna som används av klienter som ansluter till ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-172">These keys are used by the clients connecting to your cache.</span></span>

![Redis-Cache snabbtangenter](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a><span data-ttu-id="fb0f4-174">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-174">Advanced settings</span></span>
<span data-ttu-id="fb0f4-175">Följande inställningar konfigureras på den **avancerade inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-175">The following settings are configured on the **Advanced settings** blade.</span></span>

* [<span data-ttu-id="fb0f4-176">Åtkomstportar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-176">Access Ports</span></span>](#access-ports)
* [<span data-ttu-id="fb0f4-177">Principer för minne</span><span class="sxs-lookup"><span data-stu-id="fb0f4-177">Memory policies</span></span>](#memory-policies)
* [<span data-ttu-id="fb0f4-178">Keyspace-meddelanden (avancerade inställningar)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-178">Keyspace notifications (advanced settings)</span></span>](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a><span data-ttu-id="fb0f4-179">Åtkomstportar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-179">Access Ports</span></span>
<span data-ttu-id="fb0f4-180">Som standard är icke-SSL-åtkomst inaktiverad för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-180">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="fb0f4-181">Om du vill aktivera porten utan SSL **nr** för **Tillåt åtkomst endast via SSL** på den **avancerade inställningar** bladet och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-181">To enable the non-SSL port, click **No** for **Allow access only via SSL** on the **Advanced settings** blade and then click **Save**.</span></span>

![Åtkomstportar för redis-Cache](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a><span data-ttu-id="fb0f4-183">Principer för minne</span><span class="sxs-lookup"><span data-stu-id="fb0f4-183">Memory policies</span></span>
<span data-ttu-id="fb0f4-184">Den **Maxmemory princip**, **maxmemory-reserverade**, och **maxfragmentationmemory-reserverade** inställningarna på den **avancerade inställningar** bladet konfigurera principer för minne för cachen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-184">The **Maxmemory policy**, **maxmemory-reserved**, and **maxfragmentationmemory-reserved** settings on the **Advanced settings** blade configure the memory policies for the cache.</span></span>

![Redis-Cache Maxmemory princip](./media/cache-configure/redis-cache-maxmemory-policy.png)

<span data-ttu-id="fb0f4-186">**Maxmemory princip** konfigurerar av principen för cachen och gör att du kan välja mellan följande av principer:</span><span class="sxs-lookup"><span data-stu-id="fb0f4-186">**Maxmemory policy** configures the eviction policy for the cache and allows you to choose from the following eviction policies:</span></span>

* <span data-ttu-id="fb0f4-187">`volatile-lru`-Detta är standardinställningen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-187">`volatile-lru` - this is the default.</span></span>
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

<span data-ttu-id="fb0f4-188">Mer information om `maxmemory` principer, se [av principer](http://redis.io/topics/lru-cache#eviction-policies).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-188">For more information about `maxmemory` policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).</span></span>

<span data-ttu-id="fb0f4-189">Den **maxmemory-reserverade** inställningen konfigurerar hur mycket minne i MB som har reserverats för icke-cache-åtgärder som replikering under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-189">The **maxmemory-reserved** setting configures the amount of memory in MB that is reserved for non-cache operations such as replication during failover.</span></span> <span data-ttu-id="fb0f4-190">Det här värdet kan du ha en mer konsekvent användarupplevelse för Redis-servern när din belastningen varierar.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-190">Setting this value allows you to have a more consistent Redis server experience when your load varies.</span></span> <span data-ttu-id="fb0f4-191">Det här värdet ska anges för arbetsbelastningar som är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-191">This value should be set higher for workloads that are write heavy.</span></span> <span data-ttu-id="fb0f4-192">När minnet reserveras för dessa åtgärder är inte tillgänglig för lagring av cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-192">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="fb0f4-193">Den **maxfragmentationmemory-reserverade** inställningen konfigurerar hur mycket minne i MB som är reserverade för att anpassa för minnesfragmentering.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-193">The **maxfragmentationmemory-reserved** setting configures the amount of memory in MB that is reserved to accommodate for memory fragmentation.</span></span> <span data-ttu-id="fb0f4-194">Det här värdet kan du ha en mer konsekvent Redis-server när cachen är full eller nära full och fragmenteringen förhållandet också är hög.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-194">Setting this value allows you to have a more consistent Redis server experience when the cache is full or close to full and the fragmentation ratio is also high.</span></span> <span data-ttu-id="fb0f4-195">När minnet reserveras för dessa åtgärder är inte tillgänglig för lagring av cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-195">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="fb0f4-196">En sak att tänka på när du väljer ett nytt minne reservation värde (**maxmemory-reserverade** eller **maxfragmentationmemory-reserverade**) är hur den här ändringen kan påverka en cache som redan körs med stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-196">One thing to consider when choosing a new memory reservation value (**maxmemory-reserved** or **maxfragmentationmemory-reserved**) is how this change might affect a cache that is already running with large amounts of data in it.</span></span> <span data-ttu-id="fb0f4-197">Om du har en 53 GB cache med 49 GB data och sedan ändra värdet för reservation till 8 GB, till exempel kommer detta släppa max tillgängligt minne i systemet till 45 GB.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-197">For instance, if you have a 53 GB cache with 49 GB of data, then change the reservation value to 8 GB, this will drop the max available memory for the system down to 45 GB.</span></span> <span data-ttu-id="fb0f4-198">Om din aktuella `used_memory` eller `used_memory_rss` värden är högre än den nya gränsen på 45 GB och systemet måste ta bort data förrän både `used_memory` och `used_memory_rss` är lägre än 45 GB.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-198">If either your current `used_memory` or your `used_memory_rss` values are higher than the new limit of 45 GB, then the system will have to evict data until both `used_memory` and `used_memory_rss` are below 45 GB.</span></span> <span data-ttu-id="fb0f4-199">Borttagning kan öka belastningen och minne fragmentering på servern.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-199">Eviction can increase server load and memory fragmentation.</span></span> <span data-ttu-id="fb0f4-200">Mer information om cache mått som `used_memory` och `used_memory_rss`, se [tillgängliga mått och rapportering intervall](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-200">For more information on cache metrics such as `used_memory` and `used_memory_rss`, see [Available metrics and reporting intervals](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-201">Den **maxmemory-reserverade** och **maxfragmentationmemory-reserverade** inställningarna är bara tillgängliga för Standard och Premium cachelagrar.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-201">The **maxmemory-reserved** and **maxfragmentationmemory-reserved** settings are only available for Standard and Premium caches.</span></span>
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a><span data-ttu-id="fb0f4-202">Keyspace-meddelanden (avancerade inställningar)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-202">Keyspace notifications (advanced settings)</span></span>
<span data-ttu-id="fb0f4-203">Redis keyspace meddelanden har konfigurerats på den **avancerade inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-203">Redis keyspace notifications are configured on the **Advanced settings** blade.</span></span> <span data-ttu-id="fb0f4-204">Keyspace-meddelanden tillåter klienter att ta emot meddelanden när vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-204">Keyspace notifications allow clients to receive notifications when certain events occur.</span></span>

![Redis-Cache avancerade inställningar](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-206">Keyspace-meddelanden och **meddela-keyspace-händelser** inställningen är bara tillgängliga för Standard- och Premium.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-206">Keyspace notifications and the **notify-keyspace-events** setting are only available for Standard and Premium caches.</span></span>
> 
> 

<span data-ttu-id="fb0f4-207">Mer information finns i [Redis Keyspace-meddelanden](http://redis.io/topics/notifications).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-207">For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications).</span></span> <span data-ttu-id="fb0f4-208">Exempelkod finns i [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) filen i den [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-208">For sample code, see the [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in the [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.</span></span>


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a><span data-ttu-id="fb0f4-209">Redis-Cache Advisor</span><span class="sxs-lookup"><span data-stu-id="fb0f4-209">Redis Cache Advisor</span></span>
<span data-ttu-id="fb0f4-210">Den **Redis-Cache Advisor** bladet visar rekommendationer för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-210">The **Redis Cache Advisor** blade displays recommendations for your cache.</span></span> <span data-ttu-id="fb0f4-211">Inga rekommendationer visas under normal drift.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-211">During normal operations, no recommendations are displayed.</span></span> 

![Rekommendationer](./media/cache-configure/redis-cache-no-recommendations.png)

<span data-ttu-id="fb0f4-213">Om eventuella villkor som uppstår under drift av ditt cacheminne som hög minnesanvändning, nätverkets bandbredd eller belastningen på servern, en varning visas på den **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-213">If any conditions occur during the operations of your cache such as high memory usage, network bandwidth, or server load, an alert is displayed on the **Redis Cache** blade.</span></span>

![Rekommendationer](./media/cache-configure/redis-cache-recommendations-alert.png)

<span data-ttu-id="fb0f4-215">Mer information finns på den **rekommendationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-215">Further information can be found on the **Recommendations** blade.</span></span>

![Rekommendationer](./media/cache-configure/redis-cache-recommendations.png)

<span data-ttu-id="fb0f4-217">Du kan övervaka de här måtten på den [övervakning diagram](cache-how-to-monitor.md#monitoring-charts) och [användning diagram](cache-how-to-monitor.md#usage-charts) avsnitt i den **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-217">You can monitor these metrics on the [Monitoring charts](cache-how-to-monitor.md#monitoring-charts) and [Usage charts](cache-how-to-monitor.md#usage-charts) sections of the **Redis Cache** blade.</span></span>

<span data-ttu-id="fb0f4-218">Varje prisnivå har olika begränsningar för klientanslutningar, minne och bandbredd.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-218">Each pricing tier has different limits for client connections, memory, and bandwidth.</span></span> <span data-ttu-id="fb0f4-219">Om ditt cacheminne närmar sig maximal kapacitet för de här måtten över en längre period, skapas en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-219">If your cache approaches maximum capacity for these metrics over a sustained period of time, a recommendation is created.</span></span> <span data-ttu-id="fb0f4-220">Mer information om de mått och gränser som granskas av den **rekommendationer** verktyget, se tabellen nedan:</span><span class="sxs-lookup"><span data-stu-id="fb0f4-220">For more information about the metrics and limits reviewed by the **Recommendations** tool, see the following table:</span></span>

| <span data-ttu-id="fb0f4-221">Redis-Cache-mått</span><span class="sxs-lookup"><span data-stu-id="fb0f4-221">Redis Cache metric</span></span> | <span data-ttu-id="fb0f4-222">Mer information</span><span class="sxs-lookup"><span data-stu-id="fb0f4-222">More information</span></span> |
| --- | --- |
| <span data-ttu-id="fb0f4-223">Användningen av nätverksbandbredd</span><span class="sxs-lookup"><span data-stu-id="fb0f4-223">Network bandwidth usage</span></span> |[<span data-ttu-id="fb0f4-224">Prestanda för cache - bandbredd</span><span class="sxs-lookup"><span data-stu-id="fb0f4-224">Cache performance - available bandwidth</span></span>](cache-faq.md#cache-performance) |
| <span data-ttu-id="fb0f4-225">Anslutna klienter</span><span class="sxs-lookup"><span data-stu-id="fb0f4-225">Connected clients</span></span> |[<span data-ttu-id="fb0f4-226">Server för standardkonfigurationen - Redis-maxclients</span><span class="sxs-lookup"><span data-stu-id="fb0f4-226">Default Redis server configuration - maxclients</span></span>](#maxclients) |
| <span data-ttu-id="fb0f4-227">Belastningen på servern</span><span class="sxs-lookup"><span data-stu-id="fb0f4-227">Server load</span></span> |[<span data-ttu-id="fb0f4-228">Användning-diagram - Redis-serverbelastning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-228">Usage charts - Redis Server Load</span></span>](cache-how-to-monitor.md#usage-charts) |
| <span data-ttu-id="fb0f4-229">Minnesanvändning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-229">Memory usage</span></span> |[<span data-ttu-id="fb0f4-230">Prestanda för cache - storleken</span><span class="sxs-lookup"><span data-stu-id="fb0f4-230">Cache performance - size</span></span>](cache-faq.md#cache-performance) |

<span data-ttu-id="fb0f4-231">Om du vill uppgradera ditt cacheminne, klickar du på **uppgradera nu** att ändra prisnivå och [skala](#scale) ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-231">To upgrade your cache, click **Upgrade now** to change the pricing tier and [scale](#scale) your cache.</span></span> <span data-ttu-id="fb0f4-232">Mer information om hur du väljer en prisnivå finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-232">For more information on choosing a pricing tier, see [What Redis Cache offering and size should I use?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span></span>


### <a name="scale"></a><span data-ttu-id="fb0f4-233">Skala</span><span class="sxs-lookup"><span data-stu-id="fb0f4-233">Scale</span></span>
<span data-ttu-id="fb0f4-234">Klicka på **skala** att visa eller ändra prisnivå för din cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-234">Click **Scale** to view or change the pricing tier for your cache.</span></span> <span data-ttu-id="fb0f4-235">Läs mer om att skala [så skala Azure Redis-Cache](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-235">For more information on scaling, see [How to Scale Azure Redis Cache](cache-how-to-scale.md).</span></span>

![Redis-Cache prisnivån](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a><span data-ttu-id="fb0f4-237">Redis klusterstorleken</span><span class="sxs-lookup"><span data-stu-id="fb0f4-237">Redis Cluster Size</span></span>
<span data-ttu-id="fb0f4-238">Klicka på **(FÖRHANDSGRANSKNING) Redis klusterstorleken** ändra klusterstorleken för ett premium-cache med aktiverad klustring.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-238">Click **(PREVIEW) Redis Cluster Size** to change the cluster size for a running premium cache with clustering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="fb0f4-239">Observera att medan Azure Redis Cache Premium-nivån har släppts för allmän tillgänglighet, funktionen Redis Klusterstorleken är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-239">Note that while the Azure Redis Cache Premium tier has been released to General Availability, the Redis Cluster Size feature is currently in preview.</span></span>
> 
> 

![Redis klusterstorleken](./media/cache-configure/redis-cache-redis-cluster-size.png)

<span data-ttu-id="fb0f4-241">Om du vill ändra klusterstorleken skjutreglaget eller ange ett tal mellan 1 och 10 i den **Fragmentera antal** textrutan och klicka på **OK** att spara.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-241">To change the cluster size, use the slider or type a number between 1 and 10 in the **Shard count** text box and click **OK** to save.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-242">Redis-kluster är endast tillgängligt för Premium.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-242">Redis clustering is only available for Premium caches.</span></span> <span data-ttu-id="fb0f4-243">Mer information finns i [Konfigurera klustring för premium Azure Redis-cache](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-243">For more information, see [How to configure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>
> 
> 


### <a name="redis-data-persistence"></a><span data-ttu-id="fb0f4-244">Redis-datapersistence</span><span class="sxs-lookup"><span data-stu-id="fb0f4-244">Redis data persistence</span></span>
<span data-ttu-id="fb0f4-245">Klicka på **Redis-datapersistence** för att aktivera, inaktivera eller konfigurera datapersistence för premium-cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-245">Click **Redis data persistence** to enable, disable, or configure data persistence for your premium cache.</span></span> <span data-ttu-id="fb0f4-246">Azure Redis-Cache erbjuder Redis-datapersistence med antingen [RDB beständiga](cache-how-to-premium-persistence.md#configure-rdb-persistence) eller [AOF beständiga](cache-how-to-premium-persistence.md#configure-aof-persistence).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-246">Azure Redis Cache offers Redis persistence using either [RDB persistence](cache-how-to-premium-persistence.md#configure-rdb-persistence) or [AOF persistence](cache-how-to-premium-persistence.md#configure-aof-persistence).</span></span>

<span data-ttu-id="fb0f4-247">Mer information finns i [hur du konfigurerar persistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-247">For more information, see [How to configure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="fb0f4-248">Redis-datapersistence är endast tillgängligt för Premium.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-248">Redis data persistence is only available for Premium caches.</span></span> 
> 
> 

### <a name="schedule-updates"></a><span data-ttu-id="fb0f4-249">Schemauppdateringar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-249">Schedule updates</span></span>
<span data-ttu-id="fb0f4-250">Den **schemalägga uppdateringar** bladet låter dig ange en underhållsperiod för Redis-serveruppdateringar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-250">The **Schedule updates** blade allows you to designate a maintenance window for Redis server updates for your cache.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-251">Underhållsfönstret gäller endast för Redis-serveruppdateringar och inte till någon Azure-uppdateringar eller uppdateringar för operativsystemet på de virtuella datorerna som är värdar för cachen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-251">The maintenance window applies only to Redis server updates, and not to any Azure updates or updates to the operating system of the VMs that host the cache.</span></span>
> 
> 

![Schemauppdateringar](./media/cache-configure/redis-schedule-updates.png)

<span data-ttu-id="fb0f4-253">Om du vill ange en underhållsperiod de önskade dagarna och ange Starttimme för underhåll fönster för varje dag och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-253">To specify a maintenance window, check the desired days and specify the maintenance window start hour for each day, and click **OK**.</span></span> <span data-ttu-id="fb0f4-254">Observera att fönstret Underhåll i UTC.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-254">Note that the maintenance window time is in UTC.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-255">Den **schemalägga uppdateringar** funktioner är endast tillgängligt för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-255">The **Schedule updates** functionality is only available for Premium tier caches.</span></span> <span data-ttu-id="fb0f4-256">Mer information och instruktioner finns i [Azure Redis-Cache administration - Schemalägg uppdateringar](cache-administration.md#schedule-updates).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-256">For more information and instructions, see [Azure Redis Cache administration - Schedule updates](cache-administration.md#schedule-updates).</span></span>
> 
> 

### <a name="geo-replication"></a><span data-ttu-id="fb0f4-257">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="fb0f4-257">Geo-replication</span></span>

<span data-ttu-id="fb0f4-258">Den **georeplikering** bladet tillhandahåller en mekanism för att länka två instanser i Premium-nivån Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-258">The **Geo-replication** blade provides a mechanism for linking two Premium tier Azure Redis Cache instances.</span></span> <span data-ttu-id="fb0f4-259">En cache betecknas som primär länkade cacheminnet och andra som sekundär länkade cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-259">One cache is designated as the primary linked cache, and the other as the secondary linked cache.</span></span> <span data-ttu-id="fb0f4-260">Sekundär länkade cachen skrivskyddas och data som skrivs till den primära cachen replikeras till sekundär länkade cachen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-260">The secondary linked cache becomes read-only, and data written to the primary cache is replicated to the secondary linked cache.</span></span> <span data-ttu-id="fb0f4-261">Den här funktionen kan användas för att replikera en cache över Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-261">This functionality can be used to replicate a cache across Azure regions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-262">**GEO-replikering** är endast tillgängligt för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-262">**Geo-replication** is only available for Premium tier caches.</span></span> <span data-ttu-id="fb0f4-263">Mer information och instruktioner finns i [hur du konfigurerar Geo-replikering för Azure Redis-Cache](cache-how-to-geo-replication.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-263">For more information and instructions, see [How to configure Geo-replication for Azure Redis Cache](cache-how-to-geo-replication.md).</span></span>
> 
> 

### <a name="virtual-network"></a><span data-ttu-id="fb0f4-264">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="fb0f4-264">Virtual Network</span></span>
<span data-ttu-id="fb0f4-265">Den **virtuellt nätverk** avsnitt kan du konfigurera inställningar för virtuella nätverk för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-265">The **Virtual Network** section allows you to configure the virtual network settings for your cache.</span></span> <span data-ttu-id="fb0f4-266">Information om hur du skapar en premium-cache med VNET stöd och uppdaterar sina inställningar Se [hur du konfigurerar virtuella nätverket har stöd för Premium Azure Redis-Cache](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-266">For information on creating a premium cache with VNET support and updating its settings, see [How to configure Virtual Network Support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-267">Inställningarna för virtuella nätverk är bara tillgängliga för premium som har konfigurerats med stöd för virtuellt nätverk under skapande av cachen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-267">Virtual network settings are only available for premium caches that were configured with VNET support during cache creation.</span></span> 
> 
> 

### <a name="firewall"></a><span data-ttu-id="fb0f4-268">Brandvägg</span><span class="sxs-lookup"><span data-stu-id="fb0f4-268">Firewall</span></span>

<span data-ttu-id="fb0f4-269">Klicka på **brandväggen** att visa och konfigurera brandväggsregler för din Premium Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-269">Click **Firewall** to view and configure firewall rules for your Premium Azure Redis Cache.</span></span>

![Brandvägg](./media/cache-configure/redis-firewall-rules.png)

<span data-ttu-id="fb0f4-271">Du kan ange brandväggsregler med en start- och IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-271">You can specify firewall rules with a start and end IP address range.</span></span> <span data-ttu-id="fb0f4-272">Om brandväggens regler är konfigurerade kan bara klientanslutningar från de angivna IP-adressintervall ansluta till cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-272">When firewall rules are configured, only client connections from the specified IP address ranges can connect to the cache.</span></span> <span data-ttu-id="fb0f4-273">När en brandväggsregel sparas finns det en kort fördröjning innan regeln börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-273">When a firewall rule is saved there is a short delay before the rule is effective.</span></span> <span data-ttu-id="fb0f4-274">Den här fördröjningen är vanligtvis mindre än en minut.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-274">This delay is typically less than one minute.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-275">Anslutningar från Azure Redis-Cache övervakningssystem tillåts alltid, även om brandväggens regler är konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-275">Connections from Azure Redis Cache monitoring systems are always permitted, even if firewall rules are configured.</span></span>
> 
> <span data-ttu-id="fb0f4-276">Brandväggsregler är bara tillgängliga för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-276">Firewall rules are only available for Premium tier caches.</span></span>
> 
> 

### <a name="properties"></a><span data-ttu-id="fb0f4-277">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="fb0f4-277">Properties</span></span>
<span data-ttu-id="fb0f4-278">Klicka på **egenskaper** att visa information om ditt cacheminne, inklusive cache-slutpunkten och portar.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-278">Click **Properties** to view information about your cache, including the cache endpoint and ports.</span></span>

![Redis-Cache-egenskaper](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a><span data-ttu-id="fb0f4-280">Lås</span><span class="sxs-lookup"><span data-stu-id="fb0f4-280">Locks</span></span>
<span data-ttu-id="fb0f4-281">Den **låser** avsnitt kan du låsa en prenumeration, resursgrupp eller resurs för att förhindra andra användare i din organisation av misstag tas bort eller ändra viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-281">The **Locks** section allows you to lock a subscription, resource group, or resource to prevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="fb0f4-282">Mer information finns i [Låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-282">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

### <a name="automation-script"></a><span data-ttu-id="fb0f4-283">Automation-skript</span><span class="sxs-lookup"><span data-stu-id="fb0f4-283">Automation script</span></span>

<span data-ttu-id="fb0f4-284">Klicka på **automatiseringsskriptet** att skapa och exportera en mall för dina distribuerade resurser för framtida distributioner.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-284">Click **Automation script** to build and export a template of your deployed resources for future deployments.</span></span> <span data-ttu-id="fb0f4-285">Mer information om hur du arbetar med mallar finns [distribuera resurser med Azure Resource Manager-mallar](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-285">For more information about working with templates, see [Deploy resources with Azure Resource Manager templates](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

## <a name="administration-settings"></a><span data-ttu-id="fb0f4-286">Inställningar för administration</span><span class="sxs-lookup"><span data-stu-id="fb0f4-286">Administration settings</span></span>
<span data-ttu-id="fb0f4-287">Inställningarna i den **Administration** avsnitt kan du utföra följande administrativa uppgifter för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-287">The settings in the **Administration** section allow you to perform the following administrative tasks for your cache.</span></span> 

![Administration](./media/cache-configure/redis-cache-administration.png)

* [<span data-ttu-id="fb0f4-289">Importera data</span><span class="sxs-lookup"><span data-stu-id="fb0f4-289">Import data</span></span>](#importexport)
* [<span data-ttu-id="fb0f4-290">Exportera data</span><span class="sxs-lookup"><span data-stu-id="fb0f4-290">Export data</span></span>](#importexport)
* [<span data-ttu-id="fb0f4-291">Starta om</span><span class="sxs-lookup"><span data-stu-id="fb0f4-291">Reboot</span></span>](#reboot)


### <a name="importexport"></a><span data-ttu-id="fb0f4-292">Import/Export</span><span class="sxs-lookup"><span data-stu-id="fb0f4-292">Import/Export</span></span>
<span data-ttu-id="fb0f4-293">Importera och exportera är en Azure Redis-Cache data management-åtgärd, där du kan importera och exportera data i cacheminnet genom att importera och exportera en ögonblicksbild för Redis-Cache databasen (RDB) från cache premium till en sidblob i ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-293">Import/Export is an Azure Redis Cache data management operation, which allows you to import and export data in the cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache to a page blob in an Azure Storage Account.</span></span> <span data-ttu-id="fb0f4-294">Import/Export kan du migrera mellan olika instanser av Azure Redis-Cache eller fylla i cachen med data innan de används.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-294">Import/Export enables you to migrate between different Azure Redis Cache instances or populate the cache with data before use.</span></span>

<span data-ttu-id="fb0f4-295">Import kan användas för att Redis kompatibel RDB filer från en Redis-server som kör i molnet eller miljö, inklusive Redis som körs på Linux, Windows eller valfri provider som molntjänster, till exempel Amazon Web Services och andra.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-295">Import can be used to bring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="fb0f4-296">Importera data är ett enkelt sätt att skapa ett cacheminne med data i förväg.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-296">Importing data is an easy way to create a cache with pre-populated data.</span></span> <span data-ttu-id="fb0f4-297">Azure Redis-Cache under importen, laddar RDB-filer från Azure storage i minnet och infogar nycklarna i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-297">During the import process, Azure Redis Cache loads the RDB files from Azure storage into memory, and then inserts the keys into the cache.</span></span>

<span data-ttu-id="fb0f4-298">Exportera kan du exportera de data som lagras i Azure Redis-Cache för Redis-kompatibel RDB filer.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-298">Export allows you to export the data stored in Azure Redis Cache to Redis compatible RDB files.</span></span> <span data-ttu-id="fb0f4-299">Du kan använda den här funktionen för att flytta data från en Azure Redis-Cache-instans till en annan eller till en annan Redis-server.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-299">You can use this feature to move data from one Azure Redis Cache instance to another or to another Redis server.</span></span> <span data-ttu-id="fb0f4-300">Skapa en tillfällig fil på den virtuella datorn som är värd för Azure Redis-Cache server-instansen under exporten, och överföra filen till det angivna lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-300">During the export process, a temporary file is created on the VM that hosts the Azure Redis Cache server instance, and the file is uploaded to the designated storage account.</span></span> <span data-ttu-id="fb0f4-301">När exporten har slutförts med antingen en status för lyckad eller misslyckad, tas den temporära filen bort.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-301">When the export operation completes with either a status of success or failure, the temporary file is deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-302">Import/Export är endast tillgängligt för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-302">Import/Export is only available for Premium tier caches.</span></span> <span data-ttu-id="fb0f4-303">Mer information och instruktioner finns i [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-303">For more information and instructions, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

### <a name="reboot"></a><span data-ttu-id="fb0f4-304">Starta om</span><span class="sxs-lookup"><span data-stu-id="fb0f4-304">Reboot</span></span>
<span data-ttu-id="fb0f4-305">Den **omstart** bladet kan du starta om noderna i ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-305">The **Reboot** blade allows you to reboot the nodes of your cache.</span></span> <span data-ttu-id="fb0f4-306">Den här funktionen för omstart kan du testa ditt program för återhämtning om det finns ett fel i en cachenod.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-306">This reboot capability enables you to test your application for resiliency if there is a failure of a cache node.</span></span>

![Starta om](./media/cache-configure/redis-cache-reboot.png)

<span data-ttu-id="fb0f4-308">Om du har en premium-cache med aktiverad klustring, kan du välja vilka delar av cacheminnet för att starta om.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-308">If you have a premium cache with clustering enabled, you can select which shards of the cache to reboot.</span></span>

![Starta om](./media/cache-configure/redis-cache-reboot-cluster.png)

<span data-ttu-id="fb0f4-310">Starta om en eller flera noder i ditt cacheminne, Välj önskade noder och sedan **omstart**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-310">To reboot one or more nodes of your cache, select the desired nodes and click **Reboot**.</span></span> <span data-ttu-id="fb0f4-311">Om du har en premium-cache med aktiverad klustring, Välj shard(s) att starta om och klickar sedan på **omstart**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-311">If you have a premium cache with clustering enabled, select the shard(s) to reboot and then click **Reboot**.</span></span> <span data-ttu-id="fb0f4-312">Efter några minuter tillbaka på valda noder omstart och är online några minuter senare.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-312">After a few minutes, the selected node(s) reboot, and are back online a few minutes later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fb0f4-313">Starta om datorn är nu tillgänglig för alla prisnivåer.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-313">Reboot is now available for all pricing tiers.</span></span> <span data-ttu-id="fb0f4-314">Mer information och instruktioner finns i [Azure Redis-Cache administration - omstart](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-314">For more information and instructions, see [Azure Redis Cache administration - Reboot](cache-administration.md#reboot).</span></span>
> 
> 


## <a name="monitoring"></a><span data-ttu-id="fb0f4-315">Övervakning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-315">Monitoring</span></span>

<span data-ttu-id="fb0f4-316">Den **övervakning** avsnitt kan du konfigurera diagnostik- och övervakning för Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-316">The **Monitoring** section allows you to configure diagnostics and monitoring for your Redis Cache.</span></span> <span data-ttu-id="fb0f4-317">Mer information om övervakning av Azure Redis-Cache och diagnostik finns [hur du övervakar Azure Redis-Cache](cache-how-to-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-317">For more information on Azure Redis Cache monitoring and diagnostics, see [How to monitor Azure Redis Cache](cache-how-to-monitor.md).</span></span>

![Diagnostik](./media/cache-configure/redis-cache-diagnostics.png)

* [<span data-ttu-id="fb0f4-319">Redis-mått</span><span class="sxs-lookup"><span data-stu-id="fb0f4-319">Redis metrics</span></span>](#redis-metrics)
* [<span data-ttu-id="fb0f4-320">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="fb0f4-320">Alert rules</span></span>](#alert-rules)
* [<span data-ttu-id="fb0f4-321">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="fb0f4-321">Diagnostics</span></span>](#diagnostics)

### <a name="redis-metrics"></a><span data-ttu-id="fb0f4-322">Redis-mått</span><span class="sxs-lookup"><span data-stu-id="fb0f4-322">Redis metrics</span></span>
<span data-ttu-id="fb0f4-323">Klicka på **Redis mått** till [visa mått](cache-how-to-monitor.md#view-cache-metrics) för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-323">Click **Redis metrics** to [view metrics](cache-how-to-monitor.md#view-cache-metrics) for your cache.</span></span>

### <a name="alert-rules"></a><span data-ttu-id="fb0f4-324">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="fb0f4-324">Alert rules</span></span>

<span data-ttu-id="fb0f4-325">Klicka på **Varna regler** att konfigurera aviseringar baserat på Redis-Cache mått.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-325">Click **Alert rules** to configure alerts based on Redis Cache metrics.</span></span> <span data-ttu-id="fb0f4-326">Mer information finns i [aviseringar](cache-how-to-monitor.md#alerts).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-326">For more information, see [Alerts](cache-how-to-monitor.md#alerts).</span></span>

### <a name="diagnostics"></a><span data-ttu-id="fb0f4-327">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="fb0f4-327">Diagnostics</span></span>

<span data-ttu-id="fb0f4-328">Som standard cache i Azure-Monitor är [lagras i 30 dagar](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) och sedan tas bort.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-328">By default, cache metrics in Azure Monitor are [stored for 30 days](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) and then deleted.</span></span> <span data-ttu-id="fb0f4-329">För att bevara din cache-mätvärden för längre än 30 dagar, klickar du på **diagnostik** till [konfigurera lagringskontot](cache-how-to-monitor.md#export-cache-metrics) används för att lagra cache diagnostik.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-329">To persist your cache metrics for longer than 30 days, click **Diagnostics** to [configure the storage account](cache-how-to-monitor.md#export-cache-metrics) used to store cache diagnostics.</span></span>

>[!NOTE]
><span data-ttu-id="fb0f4-330">Utöver att arkivera dina cache mått till lagring, kan du också [strömma dem till en händelsehubb eller skicka dem till logganalys](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-330">In addition to archiving your cache metrics to storage, you can also [stream them to an Event hub or send them to Log Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span></span>
>
>

## <a name="support--troubleshooting-settings"></a><span data-ttu-id="fb0f4-331">Support och felsökning av inställningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-331">Support & troubleshooting settings</span></span>
<span data-ttu-id="fb0f4-332">Inställningarna i den **stöd + felsökning** avsnittet förse dig med alternativ för att lösa problem med ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-332">The settings in the **Support + troubleshooting** section provide you with options for resolving issues with your cache.</span></span>

![Stöd för + felsökning](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [<span data-ttu-id="fb0f4-334">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="fb0f4-334">Resource health</span></span>](#resource-health)
* [<span data-ttu-id="fb0f4-335">Ny supportbegäran</span><span class="sxs-lookup"><span data-stu-id="fb0f4-335">New support request</span></span>](#new-support-request)

### <a name="resource-health"></a><span data-ttu-id="fb0f4-336">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="fb0f4-336">Resource health</span></span>
<span data-ttu-id="fb0f4-337">**Resurshälsa** bevakar resurs och anger om den körs som förväntat.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-337">**Resource health** watches your resource and tells you if it's running as expected.</span></span> <span data-ttu-id="fb0f4-338">Läs mer om tjänsten för hälsotillstånd Azure-resurshanteraren [Azure Resource health översikt](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-338">For more information about the Azure Resource health service, see [Azure Resource health overview](../resource-health/resource-health-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="fb0f4-339">Resurshälsotillståndet är för närvarande inte att rapportera om hälsan för Azure Redis-Cache-instanser som finns i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-339">Resource health is currently unable to report on the health of Azure Redis Cache instances hosted in a virtual network.</span></span> <span data-ttu-id="fb0f4-340">Mer information finns i [fungerar alla cachefunktioner när värd en cachelagring i ett virtuellt nätverk?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-340">For more information, see [Do all cache features work when hosting a cache in a VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span></span>
> 
> 

### <a name="new-support-request"></a><span data-ttu-id="fb0f4-341">Ny supportbegäran</span><span class="sxs-lookup"><span data-stu-id="fb0f4-341">New support request</span></span>
<span data-ttu-id="fb0f4-342">Klicka på **ny supportbegäran** att öppna en supportbegäran för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-342">Click **New support request** to open a support request for your cache.</span></span>





## <a name="default-redis-server-configuration"></a><span data-ttu-id="fb0f4-343">Standardkonfigurationen för Redis-server</span><span class="sxs-lookup"><span data-stu-id="fb0f4-343">Default Redis server configuration</span></span>
<span data-ttu-id="fb0f4-344">Nya Azure Redis-Cache-instanserna är konfigurerade med följande Redis configuration standardvärden.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-344">New Azure Redis Cache instances are configured with the following default Redis configuration values.</span></span>

> [!NOTE]
> <span data-ttu-id="fb0f4-345">Det går inte att ändra inställningarna i det här avsnittet med hjälp av den `StackExchange.Redis.IServer.ConfigSet` metoden.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-345">The settings in this section cannot be changed using the `StackExchange.Redis.IServer.ConfigSet` method.</span></span> <span data-ttu-id="fb0f4-346">Om den här metoden anropas med något av kommandona i det här avsnittet, genereras ett undantag som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="fb0f4-346">If this method is called with one of the commands in this section, an exception similar to the following is thrown:</span></span>  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> <span data-ttu-id="fb0f4-347">Alla värden som kan konfigureras som **maximalt minne princip**, kan konfigureras via Azure-portalen eller kommandoradsverktyget hanteringsverktyg, till exempel Azure CLI eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-347">Any values that are configurable, such as **max-memory-policy**, are configurable through the Azure portal or command-line management tools such as Azure CLI or PowerShell.</span></span>
> 
> 

| <span data-ttu-id="fb0f4-348">Inställning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-348">Setting</span></span> | <span data-ttu-id="fb0f4-349">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="fb0f4-349">Default value</span></span> | <span data-ttu-id="fb0f4-350">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="fb0f4-350">Description</span></span> |
| --- | --- | --- |
| `databases` |<span data-ttu-id="fb0f4-351">16</span><span class="sxs-lookup"><span data-stu-id="fb0f4-351">16</span></span> |<span data-ttu-id="fb0f4-352">Standardantalet databaser som är 16 men du kan konfigurera ett annat antal baserat på prisnivån. <sup>1</sup> standarddatabasen är DB 0 kan du välja ett annat namn på en per anslutning grunden med hjälp av `connection.GetDatabase(dbid)` där `dbid` är ett tal mellan `0` och `databases - 1`.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-352">The default number of databases is 16 but you can configure a different number based on the pricing tier.<sup>1</sup> The default database is DB 0, you can select a different one on a per-connection basis using `connection.GetDatabase(dbid)` where `dbid` is a number between `0` and `databases - 1`.</span></span> |
| `maxclients` |<span data-ttu-id="fb0f4-353">Beror på prisnivån<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="fb0f4-353">Depends on the pricing tier<sup>2</sup></span></span> |<span data-ttu-id="fb0f4-354">Detta är det maximala antalet anslutna klienter tillåts samtidigt.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-354">This is the maximum number of connected clients allowed at the same time.</span></span> <span data-ttu-id="fb0f4-355">Gränsen har nåtts stängs Redis när alla nya anslutningar, returnera ett högsta antal klienter uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-355">Once the limit is reached Redis closes all the new connections, returning a 'max number of clients reached' error.</span></span> |
| `maxmemory-policy` |`volatile-lru` |<span data-ttu-id="fb0f4-356">Maxmemory principen är inställningen för hur Redis väljs vad du vill ta bort när `maxmemory` (storleken på cacheminnet erbjuder du valde när du skapade cachen) har nåtts.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-356">Maxmemory policy is the setting for how Redis selects what to remove when `maxmemory` (the size of the cache offering you selected when you created the cache) is reached.</span></span> <span data-ttu-id="fb0f4-357">Med Azure Redis-Cache standardinställningen är `volatile-lru`, som tar bort nycklarna med ett förfallodatum som anges med en LRU-algoritm.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-357">With Azure Redis Cache the default setting is `volatile-lru`, which removes the keys with an expiration set using an LRU algorithm.</span></span> <span data-ttu-id="fb0f4-358">Den här inställningen kan konfigureras i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-358">This setting can be configured in the Azure portal.</span></span> <span data-ttu-id="fb0f4-359">Mer information finns i [minne principer](#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-359">For more information, see [Memory policies](#memory-policies).</span></span> |
| `maxmemory-samples` |<span data-ttu-id="fb0f4-360">3</span><span class="sxs-lookup"><span data-stu-id="fb0f4-360">3</span></span> |<span data-ttu-id="fb0f4-361">Om du vill spara minne är LRU- och minimal TTL-algoritmer uppskattade algoritmer istället för exakt algoritmer.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-361">To save memory, LRU and minimal TTL algorithms are approximated algorithms instead of precise algorithms.</span></span> <span data-ttu-id="fb0f4-362">Som standard Redis kontrollerar tre nycklar och plockningar som har använts nyligen mindre.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-362">By default Redis checks three keys and picks the one that was used less recently.</span></span> |
| `lua-time-limit` |<span data-ttu-id="fb0f4-363">5,000</span><span class="sxs-lookup"><span data-stu-id="fb0f4-363">5,000</span></span> |<span data-ttu-id="fb0f4-364">Maximal körningstid för skript Lua i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-364">Max execution time of a Lua script in milliseconds.</span></span> <span data-ttu-id="fb0f4-365">Om den maximala körningstiden uppnås loggar Redis att ett skript är fortfarande i körningen efter att den maximala tillåtna tiden och börjar att besvara frågor med ett fel.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-365">If the maximum execution time is reached, Redis logs that a script is still in execution after the maximum allowed time, and starts to reply to queries with an error.</span></span> |
| `lua-event-limit` |<span data-ttu-id="fb0f4-366">500</span><span class="sxs-lookup"><span data-stu-id="fb0f4-366">500</span></span> |<span data-ttu-id="fb0f4-367">Maxstorlek på skriptet händelsekön.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-367">Max size of script event queue.</span></span> |
| <span data-ttu-id="fb0f4-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span><span class="sxs-lookup"><span data-stu-id="fb0f4-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span></span> |<span data-ttu-id="fb0f4-369">0 0 032mb 8mb 60</span><span class="sxs-lookup"><span data-stu-id="fb0f4-369">0 0 032mb 8mb 60</span></span> |<span data-ttu-id="fb0f4-370">Klienten utdata buffert gränser kan användas för att framtvinga frånkoppling av klienter som inte är att läsa data från servern tillräckligt snabbt av någon anledning (en vanlig orsak är att en Pub/Sub-klient inte kan använda meddelanden så snabbt utgivaren kan ge dem).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-370">The client output buffer limits can be used to force disconnection of clients that are not reading data from the server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as the publisher can produce them).</span></span> <span data-ttu-id="fb0f4-371">Mer information finns i [http://redis.io/topics/clients](http://redis.io/topics/clients).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-371">For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).</span></span> |

<span data-ttu-id="fb0f4-372"><a name="databases"></a>
<sup>1</sup>gränsen för `databases` är olika för varje Azure Redis-Cache prisnivån och kan anges på Skapa cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-372"><a name="databases"></a>
<sup>1</sup>The limit for `databases` is different for each Azure Redis Cache pricing tier and can be set at cache creation.</span></span> <span data-ttu-id="fb0f4-373">Om inget `databases` inställningen anges under skapande av cache standardvärdet är 16.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-373">If no `databases` setting is specified during cache creation, the default is 16.</span></span>

* <span data-ttu-id="fb0f4-374">Basic och Standard cacheminnen</span><span class="sxs-lookup"><span data-stu-id="fb0f4-374">Basic and Standard caches</span></span>
  * <span data-ttu-id="fb0f4-375">C0 (250 MB) cache - upp till 16 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-375">C0 (250 MB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="fb0f4-376">C1 (1 GB)-cache - upp till 16 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-376">C1 (1 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="fb0f4-377">C2 (2,5 GB)-cache - upp till 16 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-377">C2 (2.5 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="fb0f4-378">C3 (6 GB)-cache - upp till 16 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-378">C3 (6 GB) cache - up to 16 databases</span></span>
  * <span data-ttu-id="fb0f4-379">C4 (13 GB)-cache - upp till 32-databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-379">C4 (13 GB) cache - up to 32 databases</span></span>
  * <span data-ttu-id="fb0f4-380">C5 (26 GB)-cache - upp till 48 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-380">C5 (26 GB) cache - up to 48 databases</span></span>
  * <span data-ttu-id="fb0f4-381">C6 (53 GB)-cache - upp till 64-databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-381">C6 (53 GB) cache - up to 64 databases</span></span>
* <span data-ttu-id="fb0f4-382">Premium-cache</span><span class="sxs-lookup"><span data-stu-id="fb0f4-382">Premium caches</span></span>
  * <span data-ttu-id="fb0f4-383">P1 (6 GB - 60 GB) - upp till 16 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-383">P1 (6 GB - 60 GB) - up to 16 databases</span></span>
  * <span data-ttu-id="fb0f4-384">P2 (13 GB - 130 GB) - upp till 32-databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-384">P2 (13 GB - 130 GB) - up to 32 databases</span></span>
  * <span data-ttu-id="fb0f4-385">P3 (26 GB - 260 GB) - upp till 48 databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-385">P3 (26 GB - 260 GB) - up to 48 databases</span></span>
  * <span data-ttu-id="fb0f4-386">P4 (53 GB - 530 GB) - upp till 64-databaser</span><span class="sxs-lookup"><span data-stu-id="fb0f4-386">P4 (53 GB - 530 GB) - up to 64 databases</span></span>
  * <span data-ttu-id="fb0f4-387">Alla premium cacheminnen med Redis cluster aktiverat – Redis cluster stöder bara användning av databas 0 därför `databases` begränsa för alla premium-cache med Redis cluster aktiverat effektivt är 1 och [Välj](http://redis.io/commands/select) kommandot är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-387">All premium caches with Redis cluster enabled - Redis cluster only supports use of database 0 so the `databases` limit for any premium cache with Redis cluster enabled is effectively 1 and the [Select](http://redis.io/commands/select) command is not allowed.</span></span> <span data-ttu-id="fb0f4-388">Mer information finns i [behöver jag göra ändringar i client-program att använda kluster?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-388">For more information, see [Do I need to make any changes to my client application to use clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span></span>

<span data-ttu-id="fb0f4-389">Mer information om databaser finns [vad är Redis databaser?](cache-faq.md#what-are-redis-databases)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-389">For more information about databases, see [What are Redis databases?](cache-faq.md#what-are-redis-databases)</span></span>

> [!NOTE]
> <span data-ttu-id="fb0f4-390">Den `databases` inställning kan vara konfigurerade endast under Skapa cache och endast med hjälp av PowerShell, CLI eller annan av hanteringsklienter.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-390">The `databases` setting can be configured only during cache creation and only using PowerShell, CLI, or other management clients.</span></span> <span data-ttu-id="fb0f4-391">Ett exempel på hur du konfigurerar `databases` under Skapa cache med hjälp av PowerShell, se [ny AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-391">For an example of configuring `databases` during cache creation using PowerShell, see [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span></span>
> 
> 

<span data-ttu-id="fb0f4-392"><a name="maxclients"></a>
<sup>2</sup> `maxclients` är olika för varje Azure Redis-Cache prisnivån.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-392"><a name="maxclients"></a>
<sup>2</sup>`maxclients` is different for each Azure Redis Cache pricing tier.</span></span>

* <span data-ttu-id="fb0f4-393">Basic och Standard cacheminnen</span><span class="sxs-lookup"><span data-stu-id="fb0f4-393">Basic and Standard caches</span></span>
  * <span data-ttu-id="fb0f4-394">C0 (250 MB) cache - upp till 256-anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-394">C0 (250 MB) cache - up to 256 connections</span></span>
  * <span data-ttu-id="fb0f4-395">C1 (1 GB)-cache - upp till 1 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-395">C1 (1 GB) cache - up to 1,000 connections</span></span>
  * <span data-ttu-id="fb0f4-396">C2 (2,5 GB)-cache - upp till 2 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-396">C2 (2.5 GB) cache - up to 2,000 connections</span></span>
  * <span data-ttu-id="fb0f4-397">C3 (6 GB)-cache - upp till 5 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-397">C3 (6 GB) cache - up to 5,000 connections</span></span>
  * <span data-ttu-id="fb0f4-398">C4 (13 GB)-cache - upp till 10 000-anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-398">C4 (13 GB) cache - up to 10,000 connections</span></span>
  * <span data-ttu-id="fb0f4-399">C5 (26 GB)-cache - upp till 15 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-399">C5 (26 GB) cache - up to 15,000 connections</span></span>
  * <span data-ttu-id="fb0f4-400">C6 (53 GB)-cache - upp till 20 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-400">C6 (53 GB) cache - up to 20,000 connections</span></span>
* <span data-ttu-id="fb0f4-401">Premium-cache</span><span class="sxs-lookup"><span data-stu-id="fb0f4-401">Premium caches</span></span>
  * <span data-ttu-id="fb0f4-402">P1 (6 GB - 60 GB) - upp till 7 500-anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-402">P1 (6 GB - 60 GB) - up to 7,500 connections</span></span>
  * <span data-ttu-id="fb0f4-403">P2 (13 GB - 130 GB) - upp till 15 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-403">P2 (13 GB - 130 GB) - up to 15,000 connections</span></span>
  * <span data-ttu-id="fb0f4-404">P3 (26 GB - 260 GB) - upp till 30 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-404">P3 (26 GB - 260 GB) - up to 30,000 connections</span></span>
  * <span data-ttu-id="fb0f4-405">P4 (53 GB - 530 GB) - upp till 40 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="fb0f4-405">P4 (53 GB - 530 GB) - up to 40,000 connections</span></span>

> [!NOTE]
> <span data-ttu-id="fb0f4-406">Med varje cachens storlek kan *upp till* ett visst antal anslutningar, varje anslutning till Redis har associeras med den.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-406">While each size of cache allows *up to* a certain number of connections, each connection to Redis has overhead associated with it.</span></span> <span data-ttu-id="fb0f4-407">Ett exempel på sådana kostnader är processor- och minnesanvändning på grund av TLS/SSL-kryptering.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-407">An example of such overhead would be CPU and memory usage as a result of TLS/SSL encryption.</span></span> <span data-ttu-id="fb0f4-408">Anslutningens maximala gränsen för en given cachestorlek förutsätter en lågt belastade cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-408">The maximum connection limit for a given cache size assumes a lightly loaded cache.</span></span> <span data-ttu-id="fb0f4-409">Om att läsa in från anslutningen kostnader *plus* belastningen från Klientåtgärder överskrider kapaciteten för systemet, cachen kan ha kapacitet problem, även om du inte har överskridit gränsen för antal anslutningar för den aktuella cachestorleken.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-409">If load from connection overhead *plus* load from client operations exceeds capacity for the system, the cache can experience capacity issues even if you have not exceeded the connection limit for the current cache size.</span></span>
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a><span data-ttu-id="fb0f4-410">Redis-kommandon som inte stöds i Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="fb0f4-410">Redis commands not supported in Azure Redis Cache</span></span>
> [!IMPORTANT]
> <span data-ttu-id="fb0f4-411">Eftersom konfiguration och hantering av Azure Redis-Cache instanser hanteras av Microsoft, inaktiveras följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-411">Because configuration and management of Azure Redis Cache instances is managed by Microsoft, the following commands are disabled.</span></span> <span data-ttu-id="fb0f4-412">Om du försöker anropa dem du ett felmeddelande liknande `"(error) ERR unknown command"`.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-412">If you try to invoke them, you receive an error message similar to `"(error) ERR unknown command"`.</span></span>
> 
> * <span data-ttu-id="fb0f4-413">BGREWRITEAOF</span><span class="sxs-lookup"><span data-stu-id="fb0f4-413">BGREWRITEAOF</span></span>
> * <span data-ttu-id="fb0f4-414">BGSAVE</span><span class="sxs-lookup"><span data-stu-id="fb0f4-414">BGSAVE</span></span>
> * <span data-ttu-id="fb0f4-415">CONFIG</span><span class="sxs-lookup"><span data-stu-id="fb0f4-415">CONFIG</span></span>
> * <span data-ttu-id="fb0f4-416">FELSÖKA</span><span class="sxs-lookup"><span data-stu-id="fb0f4-416">DEBUG</span></span>
> * <span data-ttu-id="fb0f4-417">MIGRERA</span><span class="sxs-lookup"><span data-stu-id="fb0f4-417">MIGRATE</span></span>
> * <span data-ttu-id="fb0f4-418">SPARA</span><span class="sxs-lookup"><span data-stu-id="fb0f4-418">SAVE</span></span>
> * <span data-ttu-id="fb0f4-419">AVSTÄNGNING</span><span class="sxs-lookup"><span data-stu-id="fb0f4-419">SHUTDOWN</span></span>
> * <span data-ttu-id="fb0f4-420">SLAVEOF</span><span class="sxs-lookup"><span data-stu-id="fb0f4-420">SLAVEOF</span></span>
> * <span data-ttu-id="fb0f4-421">KLUSTER - kluster skriva kommandon inaktiveras, men skrivskyddad klustret kommandon tillåts.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-421">CLUSTER - Cluster write commands are disabled, but read-only Cluster commands are permitted.</span></span>
> 
> 

<span data-ttu-id="fb0f4-422">Mer information om Redis-kommandon finns [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-422">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>

## <a name="redis-console"></a><span data-ttu-id="fb0f4-423">Redis-konsolen</span><span class="sxs-lookup"><span data-stu-id="fb0f4-423">Redis console</span></span>
<span data-ttu-id="fb0f4-424">På ett säkert sätt kan du skicka kommandon till din Azure Redis-Cache-instanser som använder den **Redis-konsolen**, som är tillgänglig i Azure-portalen för alla nivåer i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-424">You can securely issue commands to your Azure Redis Cache instances using the **Redis Console**, which is available in the Azure portal for all cache tiers.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="fb0f4-425">Redis-konsolen fungerar inte med [VNET](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-425">The Redis Console does not work with [VNET](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="fb0f4-426">När din cache är en del av ett virtuellt nätverk kan bara klienter i virtuella nätverk kan komma åt cachen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-426">When your cache is part of a VNET, only clients in the VNET can access the cache.</span></span> <span data-ttu-id="fb0f4-427">Eftersom Redis-konsolen körs i din lokala webbläsare som är utanför det virtuella nätverket, kan inte den ansluta till ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-427">Because Redis Console runs in your local browser, which is outside the VNET, it can't connect to your cache.</span></span>
> - <span data-ttu-id="fb0f4-428">Inte alla Redis-kommandon stöds i Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-428">Not all Redis commands are supported in Azure Redis Cache.</span></span> <span data-ttu-id="fb0f4-429">En lista över Redis-kommandon som är inaktiverat för Azure Redis-Cache, finns i den tidigare [Redis-kommandon som inte stöds i Azure Redis-Cache](#redis-commands-not-supported-in-azure-redis-cache) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-429">For a list of Redis commands that are disabled for Azure Redis Cache, see the previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section.</span></span> <span data-ttu-id="fb0f4-430">Mer information om Redis-kommandon finns [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-430">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>
> 
> 

<span data-ttu-id="fb0f4-431">Om du vill ansluta till Redis-konsolen klickar du på **konsolen** från den **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-431">To access the Redis Console, click **Console** from the **Redis Cache** blade.</span></span>

![Redis-konsolen](./media/cache-configure/redis-console-menu.png)

<span data-ttu-id="fb0f4-433">Skriv kommandot önskade i konsolen för att utfärda kommandon mot din cache-instans.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-433">To issue commands against your cache instance, simply type the desired command into the console.</span></span>

![Redis-konsolen](./media/cache-configure/redis-console.png)


### <a name="using-the-redis-console-with-a-premium-clustered-cache"></a><span data-ttu-id="fb0f4-435">Med premium klustrade cache Redis-konsolen</span><span class="sxs-lookup"><span data-stu-id="fb0f4-435">Using the Redis Console with a premium clustered cache</span></span>

<span data-ttu-id="fb0f4-436">När cache med Redis-konsolen med en premium för, kan du skicka kommandon till en enda Fragmentera av cachen.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-436">When using the Redis Console with a premium clustered cache, you can issue commands to a single shard of the cache.</span></span> <span data-ttu-id="fb0f4-437">Om du vill utfärda ett kommando till en specifik Fragmentera först ansluta till önskad Fragmentera genom att klicka på väljaren Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-437">To issue a command to a specific shard, first connect to the desired shard by clicking it on the shard picker.</span></span>

![Redis-konsolen](./media/cache-configure/redis-console-premium-cluster.png)

<span data-ttu-id="fb0f4-439">Om du försöker komma åt en nyckel som lagras i en annan Fragmentera än anslutna Fragmentera får ett felmeddelande som liknar följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-439">If you attempt to access a key that is stored in a different shard than the connected shard, you receive an error message similar to the following message.</span></span>

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

<span data-ttu-id="fb0f4-440">I exemplet ovan är Fragmentera 1 valda Fragmentera men `myKey` finns i Fragmentera 0, vilket indikeras med den `(shard 0)` delen av ett felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-440">In the previous example, shard 1 is the selected shard, but `myKey` is located in shard 0, as indicated by the `(shard 0)` portion of the error message.</span></span> <span data-ttu-id="fb0f4-441">I det här exemplet att komma åt `myKey`Fragmentera 0 med väljaren Fragmentera och välj sedan utfärda kommandot önskade.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-441">In this example, to access `myKey`, select shard 0 using the shard picker, and then issue the desired command.</span></span>


## <a name="move-your-cache-to-a-new-subscription"></a><span data-ttu-id="fb0f4-442">Flytta ditt cacheminne till en ny prenumeration</span><span class="sxs-lookup"><span data-stu-id="fb0f4-442">Move your cache to a new subscription</span></span>
<span data-ttu-id="fb0f4-443">Du kan flytta ditt cacheminne till en ny prenumeration genom att klicka på **flytta**.</span><span class="sxs-lookup"><span data-stu-id="fb0f4-443">You can move your cache to a new subscription by clicking **Move**.</span></span>

![Flytta Redis-Cache](./media/cache-configure/redis-cache-move.png)

<span data-ttu-id="fb0f4-445">Information om hur du flyttar resurser från en resursgrupp till en annan och från en prenumeration finns [flytta resurser till en ny resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fb0f4-445">For information on moving resources from one resource group to another, and from one subscription to another, see [Move resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="fb0f4-446">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb0f4-446">Next steps</span></span>
* <span data-ttu-id="fb0f4-447">Mer information om hur du arbetar med Redis-kommandon finns [hur kan jag köra Redis kommandon?](cache-faq.md#how-can-i-run-redis-commands)</span><span class="sxs-lookup"><span data-stu-id="fb0f4-447">For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands)</span></span>

