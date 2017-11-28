---
title: aaaHow tooconfigure Azure Redis-Cache | Microsoft Docs
description: "Förstå hello Redis standardkonfigurationen för Azure Redis-Cache och lära dig hur tooconfigure din Azure Redis-Cache instanser"
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
ms.openlocfilehash: 46bffb74cdf40e0e0a99c3a83dbe06d6fe1ea65b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-azure-redis-cache"></a><span data-ttu-id="22d4f-103">Hur tooconfigure Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="22d4f-103">How tooconfigure Azure Redis Cache</span></span>
<span data-ttu-id="22d4f-104">Det här avsnittet beskrivs hur tooreview och uppdatera hello-konfiguration för Azure Redis-Cache-instanser och omfattar hello Redis server standardkonfigurationen för Azure Redis-Cache-instanser.</span><span class="sxs-lookup"><span data-stu-id="22d4f-104">This topic describes how tooreview and update hello configuration for your Azure Redis Cache instances, and covers hello default Redis server configuration for Azure Redis Cache instances.</span></span>

> [!NOTE]
> <span data-ttu-id="22d4f-105">Mer information om hur du konfigurerar och använder premium cache-funktioner finns [hur tooconfigure beständiga](cache-how-to-premium-persistence.md), [hur tooconfigure clustering](cache-how-to-premium-clustering.md), och [hur stöder tooconfigure virtuellt nätverk ](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-105">For more information on configuring and using premium cache features, see [How tooconfigure persistence](cache-how-to-premium-persistence.md), [How tooconfigure clustering](cache-how-to-premium-clustering.md), and [How tooconfigure Virtual Network support](cache-how-to-premium-vnet.md).</span></span>
> 
> 

## <a name="configure-redis-cache-settings"></a><span data-ttu-id="22d4f-106">Konfigurera inställningar för Redis-cache</span><span class="sxs-lookup"><span data-stu-id="22d4f-106">Configure Redis cache settings</span></span>
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

<span data-ttu-id="22d4f-107">Azure Redis-Cache-inställningar visas och konfigureras på hello **Redis-Cache** blad med hjälp av hello **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-107">Azure Redis Cache settings are viewed and configured on hello **Redis Cache** blade using hello **Resource Menu**.</span></span>

![Redis-Cache-inställningar](./media/cache-configure/redis-cache-settings.png)

<span data-ttu-id="22d4f-109">Du kan visa och konfigurera följande inställningar med hjälp av hello hello **resurs menyn**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-109">You can view and configure hello following settings using hello **Resource Menu**.</span></span>

* [<span data-ttu-id="22d4f-110">Översikt</span><span class="sxs-lookup"><span data-stu-id="22d4f-110">Overview</span></span>](#overview)
* [<span data-ttu-id="22d4f-111">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="22d4f-111">Activity log</span></span>](#activity-log)
* [<span data-ttu-id="22d4f-112">Åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="22d4f-112">Access control (IAM)</span></span>](#access-control-iam)
* [<span data-ttu-id="22d4f-113">Taggar</span><span class="sxs-lookup"><span data-stu-id="22d4f-113">Tags</span></span>](#tags)
* [<span data-ttu-id="22d4f-114">Diagnostisera och lösa problem</span><span class="sxs-lookup"><span data-stu-id="22d4f-114">Diagnose and solve problems</span></span>](#diagnose-and-solve-problems)
* [<span data-ttu-id="22d4f-115">Inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-115">Settings</span></span>](#settings)
    * [<span data-ttu-id="22d4f-116">Snabbtangenter</span><span class="sxs-lookup"><span data-stu-id="22d4f-116">Access keys</span></span>](#access-keys)
    * [<span data-ttu-id="22d4f-117">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-117">Advanced settings</span></span>](#advanced-settings)
    * [<span data-ttu-id="22d4f-118">Redis-Cache Advisor</span><span class="sxs-lookup"><span data-stu-id="22d4f-118">Redis Cache Advisor</span></span>](#redis-cache-advisor)
    * [<span data-ttu-id="22d4f-119">Skalning</span><span class="sxs-lookup"><span data-stu-id="22d4f-119">Scale</span></span>](#scale)
    * [<span data-ttu-id="22d4f-120">Redis klusterstorleken</span><span class="sxs-lookup"><span data-stu-id="22d4f-120">Redis cluster size</span></span>](#cluster-size)
    * [<span data-ttu-id="22d4f-121">Redis-datapersistence</span><span class="sxs-lookup"><span data-stu-id="22d4f-121">Redis data persistence</span></span>](#redis-data-persistence)
    * [<span data-ttu-id="22d4f-122">Schemauppdateringar</span><span class="sxs-lookup"><span data-stu-id="22d4f-122">Schedule updates</span></span>](#schedule-updates)
    * [<span data-ttu-id="22d4f-123">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="22d4f-123">Geo-replication</span></span>](#geo-replication)
    * [<span data-ttu-id="22d4f-124">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="22d4f-124">Virtual Network</span></span>](#virtual-network)
    * [<span data-ttu-id="22d4f-125">Brandvägg</span><span class="sxs-lookup"><span data-stu-id="22d4f-125">Firewall</span></span>](#firewall)
    * [<span data-ttu-id="22d4f-126">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="22d4f-126">Properties</span></span>](#properties)
    * [<span data-ttu-id="22d4f-127">Lås</span><span class="sxs-lookup"><span data-stu-id="22d4f-127">Locks</span></span>](#locks)
    * [<span data-ttu-id="22d4f-128">Automation-skript</span><span class="sxs-lookup"><span data-stu-id="22d4f-128">Automation script</span></span>](#automation-script)
* [<span data-ttu-id="22d4f-129">Administration</span><span class="sxs-lookup"><span data-stu-id="22d4f-129">Administration</span></span>](#administration)
    * [<span data-ttu-id="22d4f-130">Importera data</span><span class="sxs-lookup"><span data-stu-id="22d4f-130">Import data</span></span>](#importexport)
    * [<span data-ttu-id="22d4f-131">Exportera data</span><span class="sxs-lookup"><span data-stu-id="22d4f-131">Export data</span></span>](#importexport)
    * [<span data-ttu-id="22d4f-132">Starta om</span><span class="sxs-lookup"><span data-stu-id="22d4f-132">Reboot</span></span>](#reboot)
* [<span data-ttu-id="22d4f-133">Övervakning</span><span class="sxs-lookup"><span data-stu-id="22d4f-133">Monitoring</span></span>](#monitoring)
    * [<span data-ttu-id="22d4f-134">Redis-mått</span><span class="sxs-lookup"><span data-stu-id="22d4f-134">Redis metrics</span></span>](#redis-metrics)
    * [<span data-ttu-id="22d4f-135">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="22d4f-135">Alert rules</span></span>](#alert-rules)
    * [<span data-ttu-id="22d4f-136">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="22d4f-136">Diagnostics</span></span>](#diagnostics)
* [<span data-ttu-id="22d4f-137">Support och felsökning av inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-137">Support & troubleshooting settings</span></span>](#support-amp-troubleshooting-settings)
    * [<span data-ttu-id="22d4f-138">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="22d4f-138">Resource health</span></span>](#resource-health)
    * [<span data-ttu-id="22d4f-139">Ny supportbegäran</span><span class="sxs-lookup"><span data-stu-id="22d4f-139">New support request</span></span>](#new-support-request)


## <a name="overview"></a><span data-ttu-id="22d4f-140">Översikt</span><span class="sxs-lookup"><span data-stu-id="22d4f-140">Overview</span></span>

<span data-ttu-id="22d4f-141">**Översikt över** ger dig med grundläggande information om ditt cacheminne, till exempel namn, portar, prisnivå, och valda cache mått.</span><span class="sxs-lookup"><span data-stu-id="22d4f-141">**Overview** provides you with basic information about your cache, such as name, ports, pricing tier, and selected cache metrics.</span></span>

### <a name="activity-log"></a><span data-ttu-id="22d4f-142">Aktivitetslogg</span><span class="sxs-lookup"><span data-stu-id="22d4f-142">Activity log</span></span>

<span data-ttu-id="22d4f-143">Klicka på **aktivitetsloggen** tooview åtgärder som utförs på ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-143">Click **Activity log** tooview actions performed on your cache.</span></span> <span data-ttu-id="22d4f-144">Du kan också använda filtrering tooexpand den här vyn tooinclude andra resurser.</span><span class="sxs-lookup"><span data-stu-id="22d4f-144">You can also use filtering tooexpand this view tooinclude other resources.</span></span> <span data-ttu-id="22d4f-145">Mer information om hur du arbetar med granskningsloggarna finns [granskningsåtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-145">For more information on working with audit logs, see [Audit operations with Resource Manager](../azure-resource-manager/resource-group-audit.md).</span></span> <span data-ttu-id="22d4f-146">Mer information om hur du övervakar Azure Redis-Cache-händelser finns [åtgärder och aviseringar](cache-how-to-monitor.md#operations-and-alerts).</span><span class="sxs-lookup"><span data-stu-id="22d4f-146">For more information on monitoring Azure Redis Cache events, see [Operations and alerts](cache-how-to-monitor.md#operations-and-alerts).</span></span>

### <a name="access-control-iam"></a><span data-ttu-id="22d4f-147">Åtkomstkontroll (IAM)</span><span class="sxs-lookup"><span data-stu-id="22d4f-147">Access control (IAM)</span></span>

<span data-ttu-id="22d4f-148">Hej **åtkomstkontroll (IAM)** avsnittet ger stöd för rollbaserad åtkomstkontroll (RBAC) i hello Azure portal toohelp organisationer uppfyller deras åtkomst hanteringskrav bara och exakt.</span><span class="sxs-lookup"><span data-stu-id="22d4f-148">hello **Access control (IAM)** section provides support for role-based access control (RBAC) in hello Azure portal toohelp organizations meet their access management requirements simply and precisely.</span></span> <span data-ttu-id="22d4f-149">Mer information finns i [rollbaserad åtkomstkontroll i hello Azure-portalen](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-149">For more information, see [Role-based access control in hello Azure portal](../active-directory/role-based-access-control-configure.md).</span></span>

### <a name="tags"></a><span data-ttu-id="22d4f-150">Taggar</span><span class="sxs-lookup"><span data-stu-id="22d4f-150">Tags</span></span>

<span data-ttu-id="22d4f-151">Hej **taggar** avsnitt kan du ordna dina resurser.</span><span class="sxs-lookup"><span data-stu-id="22d4f-151">hello **Tags** section helps you organize your resources.</span></span> <span data-ttu-id="22d4f-152">Mer information finns i [med hjälp av taggar tooorganize resurserna i Azure](../azure-resource-manager/resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-152">For more information, see [Using tags tooorganize your Azure resources](../azure-resource-manager/resource-group-using-tags.md).</span></span>


### <a name="diagnose-and-solve-problems"></a><span data-ttu-id="22d4f-153">Diagnosticera och lösa problem</span><span class="sxs-lookup"><span data-stu-id="22d4f-153">Diagnose and solve problems</span></span>

<span data-ttu-id="22d4f-154">Klicka på **diagnostisera och lösa problem** toobe medföljer vanliga problem och strategier för att lösa dem.</span><span class="sxs-lookup"><span data-stu-id="22d4f-154">Click **Diagnose and solve problems** toobe provided with common issues and strategies for resolving them.</span></span>



## <a name="settings"></a><span data-ttu-id="22d4f-155">Inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-155">Settings</span></span>
<span data-ttu-id="22d4f-156">Hej **inställningar** avsnittet tillåter tooaccess och konfigurera följande inställningar för ditt cacheminne hello.</span><span class="sxs-lookup"><span data-stu-id="22d4f-156">hello **Settings** section allows you tooaccess and configure hello following settings for your cache.</span></span>

* [<span data-ttu-id="22d4f-157">Snabbtangenter</span><span class="sxs-lookup"><span data-stu-id="22d4f-157">Access keys</span></span>](#access-keys)
* [<span data-ttu-id="22d4f-158">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-158">Advanced settings</span></span>](#advanced-settings)
* [<span data-ttu-id="22d4f-159">Redis-Cache Advisor</span><span class="sxs-lookup"><span data-stu-id="22d4f-159">Redis Cache Advisor</span></span>](#redis-cache-advisor)
* [<span data-ttu-id="22d4f-160">Skalning</span><span class="sxs-lookup"><span data-stu-id="22d4f-160">Scale</span></span>](#scale)
* [<span data-ttu-id="22d4f-161">Redis klusterstorleken</span><span class="sxs-lookup"><span data-stu-id="22d4f-161">Redis cluster size</span></span>](#cluster-size)
* [<span data-ttu-id="22d4f-162">Redis-datapersistence</span><span class="sxs-lookup"><span data-stu-id="22d4f-162">Redis data persistence</span></span>](#redis-data-persistence)
* [<span data-ttu-id="22d4f-163">Schemauppdateringar</span><span class="sxs-lookup"><span data-stu-id="22d4f-163">Schedule updates</span></span>](#schedule-updates)
* [<span data-ttu-id="22d4f-164">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="22d4f-164">Geo-replication</span></span>](#geo-replication)
* [<span data-ttu-id="22d4f-165">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="22d4f-165">Virtual Network</span></span>](#virtual-network)
* [<span data-ttu-id="22d4f-166">Brandvägg</span><span class="sxs-lookup"><span data-stu-id="22d4f-166">Firewall</span></span>](#firewall)
* [<span data-ttu-id="22d4f-167">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="22d4f-167">Properties</span></span>](#properties)
* [<span data-ttu-id="22d4f-168">Lås</span><span class="sxs-lookup"><span data-stu-id="22d4f-168">Locks</span></span>](#locks)
* [<span data-ttu-id="22d4f-169">Automation-skript</span><span class="sxs-lookup"><span data-stu-id="22d4f-169">Automation script</span></span>](#automation-script)



### <a name="access-keys"></a><span data-ttu-id="22d4f-170">Åtkomstnycklar</span><span class="sxs-lookup"><span data-stu-id="22d4f-170">Access keys</span></span>
<span data-ttu-id="22d4f-171">Klicka på **åtkomstnycklar** tooview eller generera hello åtkomstnycklar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-171">Click **Access keys** tooview or regenerate hello access keys for your cache.</span></span> <span data-ttu-id="22d4f-172">De här nycklarna som används av hello-klienter som ansluter tooyour cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-172">These keys are used by hello clients connecting tooyour cache.</span></span>

![Redis-Cache snabbtangenter](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a><span data-ttu-id="22d4f-174">Avancerade inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-174">Advanced settings</span></span>
<span data-ttu-id="22d4f-175">hello följande inställningar konfigureras på hello **avancerade inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-175">hello following settings are configured on hello **Advanced settings** blade.</span></span>

* [<span data-ttu-id="22d4f-176">Åtkomstportar</span><span class="sxs-lookup"><span data-stu-id="22d4f-176">Access Ports</span></span>](#access-ports)
* [<span data-ttu-id="22d4f-177">Principer för minne</span><span class="sxs-lookup"><span data-stu-id="22d4f-177">Memory policies</span></span>](#memory-policies)
* [<span data-ttu-id="22d4f-178">Keyspace-meddelanden (avancerade inställningar)</span><span class="sxs-lookup"><span data-stu-id="22d4f-178">Keyspace notifications (advanced settings)</span></span>](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a><span data-ttu-id="22d4f-179">Åtkomstportar</span><span class="sxs-lookup"><span data-stu-id="22d4f-179">Access Ports</span></span>
<span data-ttu-id="22d4f-180">Som standard är icke-SSL-åtkomst inaktiverad för nya cacheminnen.</span><span class="sxs-lookup"><span data-stu-id="22d4f-180">By default, non-SSL access is disabled for new caches.</span></span> <span data-ttu-id="22d4f-181">tooenable hello icke-SSL-port, klickar du på **nr** för **Tillåt åtkomst endast via SSL** på hello **avancerade inställningar** bladet och klicka sedan på **spara**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-181">tooenable hello non-SSL port, click **No** for **Allow access only via SSL** on hello **Advanced settings** blade and then click **Save**.</span></span>

![Åtkomstportar för redis-Cache](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a><span data-ttu-id="22d4f-183">Principer för minne</span><span class="sxs-lookup"><span data-stu-id="22d4f-183">Memory policies</span></span>
<span data-ttu-id="22d4f-184">Hej **Maxmemory princip**, **maxmemory-reserverade**, och **maxfragmentationmemory-reserverade** inställningar på hello **avancerade inställningar**bladet konfigurera hello minne principer för hello-cachen.</span><span class="sxs-lookup"><span data-stu-id="22d4f-184">hello **Maxmemory policy**, **maxmemory-reserved**, and **maxfragmentationmemory-reserved** settings on hello **Advanced settings** blade configure hello memory policies for hello cache.</span></span>

![Redis-Cache Maxmemory princip](./media/cache-configure/redis-cache-maxmemory-policy.png)

<span data-ttu-id="22d4f-186">**Maxmemory princip** konfigurerar hello av principen för hello cache och låter dig toochoose från hello efter borttagning principer:</span><span class="sxs-lookup"><span data-stu-id="22d4f-186">**Maxmemory policy** configures hello eviction policy for hello cache and allows you toochoose from hello following eviction policies:</span></span>

* <span data-ttu-id="22d4f-187">`volatile-lru`-Detta är hello standard.</span><span class="sxs-lookup"><span data-stu-id="22d4f-187">`volatile-lru` - this is hello default.</span></span>
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

<span data-ttu-id="22d4f-188">Mer information om `maxmemory` principer, se [av principer](http://redis.io/topics/lru-cache#eviction-policies).</span><span class="sxs-lookup"><span data-stu-id="22d4f-188">For more information about `maxmemory` policies, see [Eviction policies](http://redis.io/topics/lru-cache#eviction-policies).</span></span>

<span data-ttu-id="22d4f-189">Hej **maxmemory-reserverade** inställningen konfigurerar hello mängden minne i MB som har reserverats för icke-cache-åtgärder som replikering under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="22d4f-189">hello **maxmemory-reserved** setting configures hello amount of memory in MB that is reserved for non-cache operations such as replication during failover.</span></span> <span data-ttu-id="22d4f-190">Det här värdet kan du toohave en mer konsekvent användarupplevelse för Redis-servern när din belastningen varierar.</span><span class="sxs-lookup"><span data-stu-id="22d4f-190">Setting this value allows you toohave a more consistent Redis server experience when your load varies.</span></span> <span data-ttu-id="22d4f-191">Det här värdet ska anges för arbetsbelastningar som är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="22d4f-191">This value should be set higher for workloads that are write heavy.</span></span> <span data-ttu-id="22d4f-192">När minnet reserveras för dessa åtgärder är inte tillgänglig för lagring av cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="22d4f-192">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="22d4f-193">Hej **maxfragmentationmemory-reserverade** inställningen konfigurerar hello mängden minne i MB som är reserverade tooaccommodate för minnesfragmentering.</span><span class="sxs-lookup"><span data-stu-id="22d4f-193">hello **maxfragmentationmemory-reserved** setting configures hello amount of memory in MB that is reserved tooaccommodate for memory fragmentation.</span></span> <span data-ttu-id="22d4f-194">Det här värdet kan du toohave en mer konsekvent användarupplevelse för Redis-servern när hello-cachen är full eller Stäng toofull och hello fragmentering förhållandet också är hög.</span><span class="sxs-lookup"><span data-stu-id="22d4f-194">Setting this value allows you toohave a more consistent Redis server experience when hello cache is full or close toofull and hello fragmentation ratio is also high.</span></span> <span data-ttu-id="22d4f-195">När minnet reserveras för dessa åtgärder är inte tillgänglig för lagring av cachelagrade data.</span><span class="sxs-lookup"><span data-stu-id="22d4f-195">When memory is reserved for such operations, it is unavailable for storage of cached data.</span></span>

<span data-ttu-id="22d4f-196">En sak tooconsider när du väljer ett nytt minne reservation värde (**maxmemory-reserverade** eller **maxfragmentationmemory-reserverade**) är hur den här ändringen kan påverka en cache som redan körs med stora mängder data.</span><span class="sxs-lookup"><span data-stu-id="22d4f-196">One thing tooconsider when choosing a new memory reservation value (**maxmemory-reserved** or **maxfragmentationmemory-reserved**) is how this change might affect a cache that is already running with large amounts of data in it.</span></span> <span data-ttu-id="22d4f-197">Om du har en 53 GB cache med 49 GB data och sedan ändra hello reservation värdet too8 GB, till exempel kommer detta släppa Hej max tillgängligt minne för hello ned too45 GB.</span><span class="sxs-lookup"><span data-stu-id="22d4f-197">For instance, if you have a 53 GB cache with 49 GB of data, then change hello reservation value too8 GB, this will drop hello max available memory for hello system down too45 GB.</span></span> <span data-ttu-id="22d4f-198">Om din aktuella `used_memory` eller `used_memory_rss` värden är högre än hello ny gräns 45 GB och sedan hello systemet har tooevict data förrän både `used_memory` och `used_memory_rss` är lägre än 45 GB.</span><span class="sxs-lookup"><span data-stu-id="22d4f-198">If either your current `used_memory` or your `used_memory_rss` values are higher than hello new limit of 45 GB, then hello system will have tooevict data until both `used_memory` and `used_memory_rss` are below 45 GB.</span></span> <span data-ttu-id="22d4f-199">Borttagning kan öka belastningen och minne fragmentering på servern.</span><span class="sxs-lookup"><span data-stu-id="22d4f-199">Eviction can increase server load and memory fragmentation.</span></span> <span data-ttu-id="22d4f-200">Mer information om cache mått som `used_memory` och `used_memory_rss`, se [tillgängliga mått och rapportering intervall](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span><span class="sxs-lookup"><span data-stu-id="22d4f-200">For more information on cache metrics such as `used_memory` and `used_memory_rss`, see [Available metrics and reporting intervals](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-201">Hej **maxmemory-reserverade** och **maxfragmentationmemory-reserverade** inställningarna är bara tillgängliga för Standard och Premium cachelagrar.</span><span class="sxs-lookup"><span data-stu-id="22d4f-201">hello **maxmemory-reserved** and **maxfragmentationmemory-reserved** settings are only available for Standard and Premium caches.</span></span>
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a><span data-ttu-id="22d4f-202">Keyspace-meddelanden (avancerade inställningar)</span><span class="sxs-lookup"><span data-stu-id="22d4f-202">Keyspace notifications (advanced settings)</span></span>
<span data-ttu-id="22d4f-203">Redis keyspace-meddelanden är konfigurerade på hello **avancerade inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-203">Redis keyspace notifications are configured on hello **Advanced settings** blade.</span></span> <span data-ttu-id="22d4f-204">Keyspace-meddelanden tillåter klienter tooreceive meddelanden när vissa händelser inträffar.</span><span class="sxs-lookup"><span data-stu-id="22d4f-204">Keyspace notifications allow clients tooreceive notifications when certain events occur.</span></span>

![Redis-Cache avancerade inställningar](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> <span data-ttu-id="22d4f-206">Keyspace-meddelanden och hello **meddela-keyspace-händelser** inställningen är bara tillgängliga för Standard- och Premium.</span><span class="sxs-lookup"><span data-stu-id="22d4f-206">Keyspace notifications and hello **notify-keyspace-events** setting are only available for Standard and Premium caches.</span></span>
> 
> 

<span data-ttu-id="22d4f-207">Mer information finns i [Redis Keyspace-meddelanden](http://redis.io/topics/notifications).</span><span class="sxs-lookup"><span data-stu-id="22d4f-207">For more information, see [Redis Keyspace Notifications](http://redis.io/topics/notifications).</span></span> <span data-ttu-id="22d4f-208">Exempelkod finns hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) filen i hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel.</span><span class="sxs-lookup"><span data-stu-id="22d4f-208">For sample code, see hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) file in hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) sample.</span></span>


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a><span data-ttu-id="22d4f-209">Redis-Cache Advisor</span><span class="sxs-lookup"><span data-stu-id="22d4f-209">Redis Cache Advisor</span></span>
<span data-ttu-id="22d4f-210">Hej **Redis-Cache Advisor** bladet visar rekommendationer för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-210">hello **Redis Cache Advisor** blade displays recommendations for your cache.</span></span> <span data-ttu-id="22d4f-211">Inga rekommendationer visas under normal drift.</span><span class="sxs-lookup"><span data-stu-id="22d4f-211">During normal operations, no recommendations are displayed.</span></span> 

![Rekommendationer](./media/cache-configure/redis-cache-no-recommendations.png)

<span data-ttu-id="22d4f-213">Om alla villkor som uppstår under hello drift av ditt cacheminne som hög minnesanvändning, nätverkets bandbredd eller belastningen på servern, visas en avisering på hello **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-213">If any conditions occur during hello operations of your cache such as high memory usage, network bandwidth, or server load, an alert is displayed on hello **Redis Cache** blade.</span></span>

![Rekommendationer](./media/cache-configure/redis-cache-recommendations-alert.png)

<span data-ttu-id="22d4f-215">Mer information finns på hello **rekommendationer** bladet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-215">Further information can be found on hello **Recommendations** blade.</span></span>

![Rekommendationer](./media/cache-configure/redis-cache-recommendations.png)

<span data-ttu-id="22d4f-217">Du kan övervaka de här måtten på hello [övervakning diagram](cache-how-to-monitor.md#monitoring-charts) och [användning diagram](cache-how-to-monitor.md#usage-charts) avsnitt i hello **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-217">You can monitor these metrics on hello [Monitoring charts](cache-how-to-monitor.md#monitoring-charts) and [Usage charts](cache-how-to-monitor.md#usage-charts) sections of hello **Redis Cache** blade.</span></span>

<span data-ttu-id="22d4f-218">Varje prisnivå har olika begränsningar för klientanslutningar, minne och bandbredd.</span><span class="sxs-lookup"><span data-stu-id="22d4f-218">Each pricing tier has different limits for client connections, memory, and bandwidth.</span></span> <span data-ttu-id="22d4f-219">Om ditt cacheminne närmar sig maximal kapacitet för de här måtten över en längre period, skapas en rekommendation.</span><span class="sxs-lookup"><span data-stu-id="22d4f-219">If your cache approaches maximum capacity for these metrics over a sustained period of time, a recommendation is created.</span></span> <span data-ttu-id="22d4f-220">Mer information om hello mått och gränser som granskas av hello **rekommendationer** verktyget, se följande tabell hello:</span><span class="sxs-lookup"><span data-stu-id="22d4f-220">For more information about hello metrics and limits reviewed by hello **Recommendations** tool, see hello following table:</span></span>

| <span data-ttu-id="22d4f-221">Redis-Cache-mått</span><span class="sxs-lookup"><span data-stu-id="22d4f-221">Redis Cache metric</span></span> | <span data-ttu-id="22d4f-222">Mer information</span><span class="sxs-lookup"><span data-stu-id="22d4f-222">More information</span></span> |
| --- | --- |
| <span data-ttu-id="22d4f-223">Användningen av nätverksbandbredd</span><span class="sxs-lookup"><span data-stu-id="22d4f-223">Network bandwidth usage</span></span> |[<span data-ttu-id="22d4f-224">Prestanda för cache - bandbredd</span><span class="sxs-lookup"><span data-stu-id="22d4f-224">Cache performance - available bandwidth</span></span>](cache-faq.md#cache-performance) |
| <span data-ttu-id="22d4f-225">Anslutna klienter</span><span class="sxs-lookup"><span data-stu-id="22d4f-225">Connected clients</span></span> |[<span data-ttu-id="22d4f-226">Server för standardkonfigurationen - Redis-maxclients</span><span class="sxs-lookup"><span data-stu-id="22d4f-226">Default Redis server configuration - maxclients</span></span>](#maxclients) |
| <span data-ttu-id="22d4f-227">Belastningen på servern</span><span class="sxs-lookup"><span data-stu-id="22d4f-227">Server load</span></span> |[<span data-ttu-id="22d4f-228">Användning-diagram - Redis-serverbelastning</span><span class="sxs-lookup"><span data-stu-id="22d4f-228">Usage charts - Redis Server Load</span></span>](cache-how-to-monitor.md#usage-charts) |
| <span data-ttu-id="22d4f-229">Minnesanvändning</span><span class="sxs-lookup"><span data-stu-id="22d4f-229">Memory usage</span></span> |[<span data-ttu-id="22d4f-230">Prestanda för cache - storleken</span><span class="sxs-lookup"><span data-stu-id="22d4f-230">Cache performance - size</span></span>](cache-faq.md#cache-performance) |

<span data-ttu-id="22d4f-231">tooupgrade ditt cacheminne, klickar du på **uppgradera nu** toochange hello prisnivån och [skala](#scale) ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-231">tooupgrade your cache, click **Upgrade now** toochange hello pricing tier and [scale](#scale) your cache.</span></span> <span data-ttu-id="22d4f-232">Mer information om hur du väljer en prisnivå finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span><span class="sxs-lookup"><span data-stu-id="22d4f-232">For more information on choosing a pricing tier, see [What Redis Cache offering and size should I use?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)</span></span>


### <a name="scale"></a><span data-ttu-id="22d4f-233">Skala</span><span class="sxs-lookup"><span data-stu-id="22d4f-233">Scale</span></span>
<span data-ttu-id="22d4f-234">Klicka på **skala** tooview eller ändra hello prisnivån för din cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-234">Click **Scale** tooview or change hello pricing tier for your cache.</span></span> <span data-ttu-id="22d4f-235">Läs mer om att skala [hur tooScale Azure Redis-Cache](cache-how-to-scale.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-235">For more information on scaling, see [How tooScale Azure Redis Cache](cache-how-to-scale.md).</span></span>

![Redis-Cache prisnivån](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a><span data-ttu-id="22d4f-237">Redis klusterstorleken</span><span class="sxs-lookup"><span data-stu-id="22d4f-237">Redis Cluster Size</span></span>
<span data-ttu-id="22d4f-238">Klicka på **(FÖRHANDSGRANSKNING) Redis klusterstorleken** toochange hello klusterstorleken körs bidrag cachelagra med aktiverad klustring.</span><span class="sxs-lookup"><span data-stu-id="22d4f-238">Click **(PREVIEW) Redis Cluster Size** toochange hello cluster size for a running premium cache with clustering enabled.</span></span>

> [!NOTE]
> <span data-ttu-id="22d4f-239">Observera att när hello Azure Redis Cache Premium nivå har publicerat tooGeneral tillgänglighet, hello Redis klusterstorleken funktionen är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="22d4f-239">Note that while hello Azure Redis Cache Premium tier has been released tooGeneral Availability, hello Redis Cluster Size feature is currently in preview.</span></span>
> 
> 

![Redis klusterstorleken](./media/cache-configure/redis-cache-redis-cluster-size.png)

<span data-ttu-id="22d4f-241">klusterstorleken för toochange hello hello skjutreglaget eller ange ett tal mellan 1 och 10 i hello **Fragmentera antal** textrutan och klicka på **OK** toosave.</span><span class="sxs-lookup"><span data-stu-id="22d4f-241">toochange hello cluster size, use hello slider or type a number between 1 and 10 in hello **Shard count** text box and click **OK** toosave.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-242">Redis-kluster är endast tillgängligt för Premium.</span><span class="sxs-lookup"><span data-stu-id="22d4f-242">Redis clustering is only available for Premium caches.</span></span> <span data-ttu-id="22d4f-243">Mer information finns i [hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-243">For more information, see [How tooconfigure clustering for a Premium Azure Redis Cache](cache-how-to-premium-clustering.md).</span></span>
> 
> 


### <a name="redis-data-persistence"></a><span data-ttu-id="22d4f-244">Redis-datapersistence</span><span class="sxs-lookup"><span data-stu-id="22d4f-244">Redis data persistence</span></span>
<span data-ttu-id="22d4f-245">Klicka på **Redis-datapersistence** tooenable, inaktivera eller konfigurera datapersistence för premium-cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-245">Click **Redis data persistence** tooenable, disable, or configure data persistence for your premium cache.</span></span> <span data-ttu-id="22d4f-246">Azure Redis-Cache erbjuder Redis-datapersistence med antingen [RDB beständiga](cache-how-to-premium-persistence.md#configure-rdb-persistence) eller [AOF beständiga](cache-how-to-premium-persistence.md#configure-aof-persistence).</span><span class="sxs-lookup"><span data-stu-id="22d4f-246">Azure Redis Cache offers Redis persistence using either [RDB persistence](cache-how-to-premium-persistence.md#configure-rdb-persistence) or [AOF persistence](cache-how-to-premium-persistence.md#configure-aof-persistence).</span></span>

<span data-ttu-id="22d4f-247">Mer information finns i [hur tooconfigure persistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-247">For more information, see [How tooconfigure persistence for a Premium Azure Redis Cache](cache-how-to-premium-persistence.md).</span></span>


> [!IMPORTANT]
> <span data-ttu-id="22d4f-248">Redis-datapersistence är endast tillgängligt för Premium.</span><span class="sxs-lookup"><span data-stu-id="22d4f-248">Redis data persistence is only available for Premium caches.</span></span> 
> 
> 

### <a name="schedule-updates"></a><span data-ttu-id="22d4f-249">Schemauppdateringar</span><span class="sxs-lookup"><span data-stu-id="22d4f-249">Schedule updates</span></span>
<span data-ttu-id="22d4f-250">Hej **schemalägga uppdateringar** bladet kan du toodesignate en underhållsperiod för Redis-serveruppdateringar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-250">hello **Schedule updates** blade allows you toodesignate a maintenance window for Redis server updates for your cache.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="22d4f-251">Hej underhållsperioden gäller endast uppdateringar för tooRedis server och inte tooany Azure uppdaterar eller uppdaterar toohello operativsystemet hello virtuella datorer som är värdar för hello cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-251">hello maintenance window applies only tooRedis server updates, and not tooany Azure updates or updates toohello operating system of hello VMs that host hello cache.</span></span>
> 
> 

![Schemauppdateringar](./media/cache-configure/redis-schedule-updates.png)

<span data-ttu-id="22d4f-253">toospecify en underhållsperiod hello önskad dagar och ange hello fönstret starttid för underhåll för varje dag och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-253">toospecify a maintenance window, check hello desired days and specify hello maintenance window start hour for each day, and click **OK**.</span></span> <span data-ttu-id="22d4f-254">Observera att hello underhållsfönstertiden i UTC.</span><span class="sxs-lookup"><span data-stu-id="22d4f-254">Note that hello maintenance window time is in UTC.</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="22d4f-255">Hej **schemalägga uppdateringar** funktioner är endast tillgängligt för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="22d4f-255">hello **Schedule updates** functionality is only available for Premium tier caches.</span></span> <span data-ttu-id="22d4f-256">Mer information och instruktioner finns i [Azure Redis-Cache administration - Schemalägg uppdateringar](cache-administration.md#schedule-updates).</span><span class="sxs-lookup"><span data-stu-id="22d4f-256">For more information and instructions, see [Azure Redis Cache administration - Schedule updates](cache-administration.md#schedule-updates).</span></span>
> 
> 

### <a name="geo-replication"></a><span data-ttu-id="22d4f-257">Geo-replikering</span><span class="sxs-lookup"><span data-stu-id="22d4f-257">Geo-replication</span></span>

<span data-ttu-id="22d4f-258">Hej **georeplikering** bladet tillhandahåller en mekanism för att länka två instanser i Premium-nivån Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-258">hello **Geo-replication** blade provides a mechanism for linking two Premium tier Azure Redis Cache instances.</span></span> <span data-ttu-id="22d4f-259">En cache utses hello primära länkade cache och hello andra som sekundär länkade hello-cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-259">One cache is designated as hello primary linked cache, and hello other as hello secondary linked cache.</span></span> <span data-ttu-id="22d4f-260">hello sekundär länkade cache skrivskyddas och data som skrivits toohello primära cache är replikeras toohello sekundära länkade cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-260">hello secondary linked cache becomes read-only, and data written toohello primary cache is replicated toohello secondary linked cache.</span></span> <span data-ttu-id="22d4f-261">Den här funktionen kan vara används tooreplicate en cache i Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="22d4f-261">This functionality can be used tooreplicate a cache across Azure regions.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-262">**GEO-replikering** är endast tillgängligt för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="22d4f-262">**Geo-replication** is only available for Premium tier caches.</span></span> <span data-ttu-id="22d4f-263">Mer information och instruktioner finns i [hur tooconfigure Geo-replikering för Azure Redis-Cache](cache-how-to-geo-replication.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-263">For more information and instructions, see [How tooconfigure Geo-replication for Azure Redis Cache](cache-how-to-geo-replication.md).</span></span>
> 
> 

### <a name="virtual-network"></a><span data-ttu-id="22d4f-264">Virtual Network</span><span class="sxs-lookup"><span data-stu-id="22d4f-264">Virtual Network</span></span>
<span data-ttu-id="22d4f-265">Hej **virtuellt nätverk** avsnittet kan du tooconfigure hello virtuella nätverksinställningar för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-265">hello **Virtual Network** section allows you tooconfigure hello virtual network settings for your cache.</span></span> <span data-ttu-id="22d4f-266">Information om hur du skapar en premium-cache med VNET stöd och uppdaterar sina inställningar Se [hur tooconfigure för virtuella nätverk som har stöd för Premium Azure Redis-Cache](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-266">For information on creating a premium cache with VNET support and updating its settings, see [How tooconfigure Virtual Network Support for a Premium Azure Redis Cache](cache-how-to-premium-vnet.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-267">Inställningarna för virtuella nätverk är bara tillgängliga för premium som har konfigurerats med stöd för virtuellt nätverk under skapande av cachen.</span><span class="sxs-lookup"><span data-stu-id="22d4f-267">Virtual network settings are only available for premium caches that were configured with VNET support during cache creation.</span></span> 
> 
> 

### <a name="firewall"></a><span data-ttu-id="22d4f-268">Brandvägg</span><span class="sxs-lookup"><span data-stu-id="22d4f-268">Firewall</span></span>

<span data-ttu-id="22d4f-269">Klicka på **brandväggen** tooview och konfigurera brandväggsregler för din Premium Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-269">Click **Firewall** tooview and configure firewall rules for your Premium Azure Redis Cache.</span></span>

![Brandvägg](./media/cache-configure/redis-firewall-rules.png)

<span data-ttu-id="22d4f-271">Du kan ange brandväggsregler med en start- och IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="22d4f-271">You can specify firewall rules with a start and end IP address range.</span></span> <span data-ttu-id="22d4f-272">När brandväggsregler har konfigurerats, ange bara klientanslutningar från hello IP-adressintervall kan ansluta toohello cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-272">When firewall rules are configured, only client connections from hello specified IP address ranges can connect toohello cache.</span></span> <span data-ttu-id="22d4f-273">När en brandväggsregel sparas finns det en kort fördröjning innan hello regel börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="22d4f-273">When a firewall rule is saved there is a short delay before hello rule is effective.</span></span> <span data-ttu-id="22d4f-274">Den här fördröjningen är vanligtvis mindre än en minut.</span><span class="sxs-lookup"><span data-stu-id="22d4f-274">This delay is typically less than one minute.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-275">Anslutningar från Azure Redis-Cache övervakningssystem tillåts alltid, även om brandväggens regler är konfigurerade.</span><span class="sxs-lookup"><span data-stu-id="22d4f-275">Connections from Azure Redis Cache monitoring systems are always permitted, even if firewall rules are configured.</span></span>
> 
> <span data-ttu-id="22d4f-276">Brandväggsregler är bara tillgängliga för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="22d4f-276">Firewall rules are only available for Premium tier caches.</span></span>
> 
> 

### <a name="properties"></a><span data-ttu-id="22d4f-277">Egenskaper</span><span class="sxs-lookup"><span data-stu-id="22d4f-277">Properties</span></span>
<span data-ttu-id="22d4f-278">Klicka på **egenskaper** tooview information om ditt cacheminne, inklusive hello cache-slutpunkten och portar.</span><span class="sxs-lookup"><span data-stu-id="22d4f-278">Click **Properties** tooview information about your cache, including hello cache endpoint and ports.</span></span>

![Redis-Cache-egenskaper](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a><span data-ttu-id="22d4f-280">Lås</span><span class="sxs-lookup"><span data-stu-id="22d4f-280">Locks</span></span>
<span data-ttu-id="22d4f-281">Hej **låser** avsnittet kan du toolock en prenumeration, resursgrupp eller resurs tooprevent andra användare i din organisation av misstag tas bort eller ändra viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="22d4f-281">hello **Locks** section allows you toolock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="22d4f-282">Mer information finns i [Låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-282">For more information, see [Lock resources with Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).</span></span>

### <a name="automation-script"></a><span data-ttu-id="22d4f-283">Automation-skript</span><span class="sxs-lookup"><span data-stu-id="22d4f-283">Automation script</span></span>

<span data-ttu-id="22d4f-284">Klicka på **automatiseringsskriptet** toobuild och exportera en mall för dina distribuerade resurser för framtida distributioner.</span><span class="sxs-lookup"><span data-stu-id="22d4f-284">Click **Automation script** toobuild and export a template of your deployed resources for future deployments.</span></span> <span data-ttu-id="22d4f-285">Mer information om hur du arbetar med mallar finns [distribuera resurser med Azure Resource Manager-mallar](../azure-resource-manager/resource-group-template-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-285">For more information about working with templates, see [Deploy resources with Azure Resource Manager templates](../azure-resource-manager/resource-group-template-deploy.md).</span></span>

## <a name="administration-settings"></a><span data-ttu-id="22d4f-286">Inställningar för administration</span><span class="sxs-lookup"><span data-stu-id="22d4f-286">Administration settings</span></span>
<span data-ttu-id="22d4f-287">Hej inställningar i hello **Administration** avsnittet tillåter tooperform hello följande administrativa uppgifter för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-287">hello settings in hello **Administration** section allow you tooperform hello following administrative tasks for your cache.</span></span> 

![Administration](./media/cache-configure/redis-cache-administration.png)

* [<span data-ttu-id="22d4f-289">Importera data</span><span class="sxs-lookup"><span data-stu-id="22d4f-289">Import data</span></span>](#importexport)
* [<span data-ttu-id="22d4f-290">Exportera data</span><span class="sxs-lookup"><span data-stu-id="22d4f-290">Export data</span></span>](#importexport)
* [<span data-ttu-id="22d4f-291">Starta om</span><span class="sxs-lookup"><span data-stu-id="22d4f-291">Reboot</span></span>](#reboot)


### <a name="importexport"></a><span data-ttu-id="22d4f-292">Import/Export</span><span class="sxs-lookup"><span data-stu-id="22d4f-292">Import/Export</span></span>
<span data-ttu-id="22d4f-293">Importera och exportera är en Azure Redis-Cache data management-åtgärd, som gör att du tooimport och exportera data i hello-cachen genom att importera och exportera en ögonblicksbild för Redis-Cache databasen (RDB) från en premium cache tooa sidblob i ett Azure Storage-konto.</span><span class="sxs-lookup"><span data-stu-id="22d4f-293">Import/Export is an Azure Redis Cache data management operation, which allows you tooimport and export data in hello cache by importing and exporting a Redis Cache Database (RDB) snapshot from a premium cache tooa page blob in an Azure Storage Account.</span></span> <span data-ttu-id="22d4f-294">Import/Export kan du toomigrate mellan olika instanser av Azure Redis-Cache eller fylla hello cachen med data innan de används.</span><span class="sxs-lookup"><span data-stu-id="22d4f-294">Import/Export enables you toomigrate between different Azure Redis Cache instances or populate hello cache with data before use.</span></span>

<span data-ttu-id="22d4f-295">Importera kan vara används toobring Redis kompatibel RDB filer från en Redis-server som kör i molnet eller miljö, inklusive Redis som körs på Linux, Windows eller valfri provider som molntjänster, till exempel Amazon Web Services och andra.</span><span class="sxs-lookup"><span data-stu-id="22d4f-295">Import can be used toobring Redis compatible RDB files from any Redis server running in any cloud or environment, including Redis running on Linux, Windows, or any cloud provider such as Amazon Web Services and others.</span></span> <span data-ttu-id="22d4f-296">Importera data är ett enkelt sätt toocreate en cache med data i förväg.</span><span class="sxs-lookup"><span data-stu-id="22d4f-296">Importing data is an easy way toocreate a cache with pre-populated data.</span></span> <span data-ttu-id="22d4f-297">Azure Redis-Cache under hello importen läser in hello RDB filer från Azure storage i minnet och infogar hello nycklar i hello cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-297">During hello import process, Azure Redis Cache loads hello RDB files from Azure storage into memory, and then inserts hello keys into hello cache.</span></span>

<span data-ttu-id="22d4f-298">Exportera kan tooexport hello data som lagras i Azure Redis-Cache tooRedis kompatibel RDB filer.</span><span class="sxs-lookup"><span data-stu-id="22d4f-298">Export allows you tooexport hello data stored in Azure Redis Cache tooRedis compatible RDB files.</span></span> <span data-ttu-id="22d4f-299">Du kan använda den här funktionen toomove data från en Azure Redis-Cache-instansen tooanother eller tooanother Redis-servern.</span><span class="sxs-lookup"><span data-stu-id="22d4f-299">You can use this feature toomove data from one Azure Redis Cache instance tooanother or tooanother Redis server.</span></span> <span data-ttu-id="22d4f-300">Under exportprocessen hello skapas en temporär fil på hello VM att värdar hello Azure Redis-Cache server-instansen och hello fil överförda toohello avses storage-konto.</span><span class="sxs-lookup"><span data-stu-id="22d4f-300">During hello export process, a temporary file is created on hello VM that hosts hello Azure Redis Cache server instance, and hello file is uploaded toohello designated storage account.</span></span> <span data-ttu-id="22d4f-301">När hello exporten har slutförts med antingen en status för lyckad eller misslyckad tas hello tillfälliga filen bort.</span><span class="sxs-lookup"><span data-stu-id="22d4f-301">When hello export operation completes with either a status of success or failure, hello temporary file is deleted.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-302">Import/Export är endast tillgängligt för Premium-nivån.</span><span class="sxs-lookup"><span data-stu-id="22d4f-302">Import/Export is only available for Premium tier caches.</span></span> <span data-ttu-id="22d4f-303">Mer information och instruktioner finns i [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-303">For more information and instructions, see [Import and Export data in Azure Redis Cache](cache-how-to-import-export-data.md).</span></span>
> 
> 

### <a name="reboot"></a><span data-ttu-id="22d4f-304">Starta om</span><span class="sxs-lookup"><span data-stu-id="22d4f-304">Reboot</span></span>
<span data-ttu-id="22d4f-305">Hej **omstart** bladet kan du tooreboot hello noder i ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-305">hello **Reboot** blade allows you tooreboot hello nodes of your cache.</span></span> <span data-ttu-id="22d4f-306">Den här funktionen för omstart kan du tootest ditt program för återhämtning om det finns ett fel i en cachenod.</span><span class="sxs-lookup"><span data-stu-id="22d4f-306">This reboot capability enables you tootest your application for resiliency if there is a failure of a cache node.</span></span>

![Starta om](./media/cache-configure/redis-cache-reboot.png)

<span data-ttu-id="22d4f-308">Om du har en premium-cache med aktiverad klustring, kan du välja vilka delar av hello cache tooreboot.</span><span class="sxs-lookup"><span data-stu-id="22d4f-308">If you have a premium cache with clustering enabled, you can select which shards of hello cache tooreboot.</span></span>

![Starta om](./media/cache-configure/redis-cache-reboot-cluster.png)

<span data-ttu-id="22d4f-310">tooreboot en eller flera noder i ditt cacheminne, Välj önskad hello noder och klicka på **omstart**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-310">tooreboot one or more nodes of your cache, select hello desired nodes and click **Reboot**.</span></span> <span data-ttu-id="22d4f-311">Om du har en premium-cache med aktiverad klustring, Välj hello shard(s) tooreboot och klicka sedan på **omstart**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-311">If you have a premium cache with clustering enabled, select hello shard(s) tooreboot and then click **Reboot**.</span></span> <span data-ttu-id="22d4f-312">Hej valda noder omstart efter några minuter och är tillbaka online och ett par minuter senare.</span><span class="sxs-lookup"><span data-stu-id="22d4f-312">After a few minutes, hello selected node(s) reboot, and are back online a few minutes later.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="22d4f-313">Starta om datorn är nu tillgänglig för alla prisnivåer.</span><span class="sxs-lookup"><span data-stu-id="22d4f-313">Reboot is now available for all pricing tiers.</span></span> <span data-ttu-id="22d4f-314">Mer information och instruktioner finns i [Azure Redis-Cache administration - omstart](cache-administration.md#reboot).</span><span class="sxs-lookup"><span data-stu-id="22d4f-314">For more information and instructions, see [Azure Redis Cache administration - Reboot](cache-administration.md#reboot).</span></span>
> 
> 


## <a name="monitoring"></a><span data-ttu-id="22d4f-315">Övervakning</span><span class="sxs-lookup"><span data-stu-id="22d4f-315">Monitoring</span></span>

<span data-ttu-id="22d4f-316">Hej **övervakning** avsnittet kan du tooconfigure diagnostik- och övervakning för Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-316">hello **Monitoring** section allows you tooconfigure diagnostics and monitoring for your Redis Cache.</span></span> <span data-ttu-id="22d4f-317">Mer information om övervakning av Azure Redis-Cache och diagnostik finns [hur toomonitor Azure Redis-Cache](cache-how-to-monitor.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-317">For more information on Azure Redis Cache monitoring and diagnostics, see [How toomonitor Azure Redis Cache](cache-how-to-monitor.md).</span></span>

![Diagnostik](./media/cache-configure/redis-cache-diagnostics.png)

* [<span data-ttu-id="22d4f-319">Redis-mått</span><span class="sxs-lookup"><span data-stu-id="22d4f-319">Redis metrics</span></span>](#redis-metrics)
* [<span data-ttu-id="22d4f-320">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="22d4f-320">Alert rules</span></span>](#alert-rules)
* [<span data-ttu-id="22d4f-321">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="22d4f-321">Diagnostics</span></span>](#diagnostics)

### <a name="redis-metrics"></a><span data-ttu-id="22d4f-322">Redis-mått</span><span class="sxs-lookup"><span data-stu-id="22d4f-322">Redis metrics</span></span>
<span data-ttu-id="22d4f-323">Klicka på **Redis mått** för[visa mått](cache-how-to-monitor.md#view-cache-metrics) för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-323">Click **Redis metrics** too[view metrics](cache-how-to-monitor.md#view-cache-metrics) for your cache.</span></span>

### <a name="alert-rules"></a><span data-ttu-id="22d4f-324">Aviseringsregler</span><span class="sxs-lookup"><span data-stu-id="22d4f-324">Alert rules</span></span>

<span data-ttu-id="22d4f-325">Klicka på **Varna regler** tooconfigure aviseringar baserat på Redis-Cache mått.</span><span class="sxs-lookup"><span data-stu-id="22d4f-325">Click **Alert rules** tooconfigure alerts based on Redis Cache metrics.</span></span> <span data-ttu-id="22d4f-326">Mer information finns i [aviseringar](cache-how-to-monitor.md#alerts).</span><span class="sxs-lookup"><span data-stu-id="22d4f-326">For more information, see [Alerts](cache-how-to-monitor.md#alerts).</span></span>

### <a name="diagnostics"></a><span data-ttu-id="22d4f-327">Diagnostik</span><span class="sxs-lookup"><span data-stu-id="22d4f-327">Diagnostics</span></span>

<span data-ttu-id="22d4f-328">Som standard cache i Azure-Monitor är [lagras i 30 dagar](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) och sedan tas bort.</span><span class="sxs-lookup"><span data-stu-id="22d4f-328">By default, cache metrics in Azure Monitor are [stored for 30 days](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) and then deleted.</span></span> <span data-ttu-id="22d4f-329">toopersist din cache-mätvärden för längre än 30 dagar, klicka på **diagnostik** för[konfigurera hello lagringskonto](cache-how-to-monitor.md#export-cache-metrics) används toostore cache diagnostik.</span><span class="sxs-lookup"><span data-stu-id="22d4f-329">toopersist your cache metrics for longer than 30 days, click **Diagnostics** too[configure hello storage account](cache-how-to-monitor.md#export-cache-metrics) used toostore cache diagnostics.</span></span>

>[!NOTE]
><span data-ttu-id="22d4f-330">I tillägg tooarchiving din mått toostorage cache kan du också [strömma dem tooan Event hub eller skicka dem tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span><span class="sxs-lookup"><span data-stu-id="22d4f-330">In addition tooarchiving your cache metrics toostorage, you can also [stream them tooan Event hub or send them tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).</span></span>
>
>

## <a name="support--troubleshooting-settings"></a><span data-ttu-id="22d4f-331">Support och felsökning av inställningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-331">Support & troubleshooting settings</span></span>
<span data-ttu-id="22d4f-332">Hej inställningar i hello **stöd + felsökning** avsnittet förse dig med alternativ för att lösa problem med ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-332">hello settings in hello **Support + troubleshooting** section provide you with options for resolving issues with your cache.</span></span>

![Stöd för + felsökning](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [<span data-ttu-id="22d4f-334">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="22d4f-334">Resource health</span></span>](#resource-health)
* [<span data-ttu-id="22d4f-335">Ny supportbegäran</span><span class="sxs-lookup"><span data-stu-id="22d4f-335">New support request</span></span>](#new-support-request)

### <a name="resource-health"></a><span data-ttu-id="22d4f-336">Resurshälsa</span><span class="sxs-lookup"><span data-stu-id="22d4f-336">Resource health</span></span>
<span data-ttu-id="22d4f-337">**Resurshälsa** bevakar resurs och anger om den körs som förväntat.</span><span class="sxs-lookup"><span data-stu-id="22d4f-337">**Resource health** watches your resource and tells you if it's running as expected.</span></span> <span data-ttu-id="22d4f-338">Läs mer om hello Azure-resurshanteraren hälsotjänsten [Azure Resource health översikt](../resource-health/resource-health-overview.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-338">For more information about hello Azure Resource health service, see [Azure Resource health overview](../resource-health/resource-health-overview.md).</span></span>

> [!NOTE]
> <span data-ttu-id="22d4f-339">Resurshälsotillståndet är för närvarande inte tooreport på hello hälsotillstånd Azure Redis-Cache-instanser som finns i ett virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="22d4f-339">Resource health is currently unable tooreport on hello health of Azure Redis Cache instances hosted in a virtual network.</span></span> <span data-ttu-id="22d4f-340">Mer information finns i [fungerar alla cachefunktioner när värd en cachelagring i ett virtuellt nätverk?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span><span class="sxs-lookup"><span data-stu-id="22d4f-340">For more information, see [Do all cache features work when hosting a cache in a VNET?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)</span></span>
> 
> 

### <a name="new-support-request"></a><span data-ttu-id="22d4f-341">Ny supportbegäran</span><span class="sxs-lookup"><span data-stu-id="22d4f-341">New support request</span></span>
<span data-ttu-id="22d4f-342">Klicka på **ny supportbegäran** tooopen en supportförfrågan för ditt cacheminne.</span><span class="sxs-lookup"><span data-stu-id="22d4f-342">Click **New support request** tooopen a support request for your cache.</span></span>





## <a name="default-redis-server-configuration"></a><span data-ttu-id="22d4f-343">Standardkonfigurationen för Redis-server</span><span class="sxs-lookup"><span data-stu-id="22d4f-343">Default Redis server configuration</span></span>
<span data-ttu-id="22d4f-344">Nya Azure Redis-Cache-instanserna är konfigurerade med hello följande standardvärden för Redis-konfigurering.</span><span class="sxs-lookup"><span data-stu-id="22d4f-344">New Azure Redis Cache instances are configured with hello following default Redis configuration values.</span></span>

> [!NOTE]
> <span data-ttu-id="22d4f-345">hello inställningar i det här avsnittet kan inte ändras med hjälp av hello `StackExchange.Redis.IServer.ConfigSet` metod.</span><span class="sxs-lookup"><span data-stu-id="22d4f-345">hello settings in this section cannot be changed using hello `StackExchange.Redis.IServer.ConfigSet` method.</span></span> <span data-ttu-id="22d4f-346">Om den här metoden anropas med en hello kommandona i det här avsnittet, genereras ett undantag liknande toohello följande:</span><span class="sxs-lookup"><span data-stu-id="22d4f-346">If this method is called with one of hello commands in this section, an exception similar toohello following is thrown:</span></span>  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> <span data-ttu-id="22d4f-347">Alla värden som kan konfigureras som **maximalt minne princip**, kan konfigureras via hello Azure-portalen eller kommandoradsverktyget hanteringsverktyg, till exempel Azure CLI eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="22d4f-347">Any values that are configurable, such as **max-memory-policy**, are configurable through hello Azure portal or command-line management tools such as Azure CLI or PowerShell.</span></span>
> 
> 

| <span data-ttu-id="22d4f-348">Inställning</span><span class="sxs-lookup"><span data-stu-id="22d4f-348">Setting</span></span> | <span data-ttu-id="22d4f-349">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="22d4f-349">Default value</span></span> | <span data-ttu-id="22d4f-350">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="22d4f-350">Description</span></span> |
| --- | --- | --- |
| `databases` |<span data-ttu-id="22d4f-351">16</span><span class="sxs-lookup"><span data-stu-id="22d4f-351">16</span></span> |<span data-ttu-id="22d4f-352">hello standardantalet databaser är 16 men du kan konfigurera ett annat nummer baserat på hello som prisnivå. <sup>1</sup> hello standarddatabasen är DB 0 kan du välja ett annat namn på en per anslutning grunden med hjälp av `connection.GetDatabase(dbid)` där `dbid` är ett tal mellan `0` och `databases - 1`.</span><span class="sxs-lookup"><span data-stu-id="22d4f-352">hello default number of databases is 16 but you can configure a different number based on hello pricing tier.<sup>1</sup> hello default database is DB 0, you can select a different one on a per-connection basis using `connection.GetDatabase(dbid)` where `dbid` is a number between `0` and `databases - 1`.</span></span> |
| `maxclients` |<span data-ttu-id="22d4f-353">Beror på hello prisnivån<sup>2</sup></span><span class="sxs-lookup"><span data-stu-id="22d4f-353">Depends on hello pricing tier<sup>2</sup></span></span> |<span data-ttu-id="22d4f-354">Detta är hello högsta antal anslutna klienter tillåts vid hello samma tid.</span><span class="sxs-lookup"><span data-stu-id="22d4f-354">This is hello maximum number of connected clients allowed at hello same time.</span></span> <span data-ttu-id="22d4f-355">Hello gränsen har nåtts stängs Redis när alla hello nya anslutningar, returneras ett fel om 'max antal klienter som nått'.</span><span class="sxs-lookup"><span data-stu-id="22d4f-355">Once hello limit is reached Redis closes all hello new connections, returning a 'max number of clients reached' error.</span></span> |
| `maxmemory-policy` |`volatile-lru` |<span data-ttu-id="22d4f-356">Princip för Maxmemory är hello inställning för hur Redis väljer vilka tooremove när `maxmemory` (hello storlek hello cache erbjuder du valde när du skapade hello cache) har nåtts.</span><span class="sxs-lookup"><span data-stu-id="22d4f-356">Maxmemory policy is hello setting for how Redis selects what tooremove when `maxmemory` (hello size of hello cache offering you selected when you created hello cache) is reached.</span></span> <span data-ttu-id="22d4f-357">Med Azure Redis-Cache hello standard är inställningen `volatile-lru`, som tar bort hello nycklar med ett förfallodatum som anges med en LRU-algoritm.</span><span class="sxs-lookup"><span data-stu-id="22d4f-357">With Azure Redis Cache hello default setting is `volatile-lru`, which removes hello keys with an expiration set using an LRU algorithm.</span></span> <span data-ttu-id="22d4f-358">Den här inställningen kan konfigureras i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="22d4f-358">This setting can be configured in hello Azure portal.</span></span> <span data-ttu-id="22d4f-359">Mer information finns i [minne principer](#memory-policies).</span><span class="sxs-lookup"><span data-stu-id="22d4f-359">For more information, see [Memory policies](#memory-policies).</span></span> |
| `maxmemory-samples` |<span data-ttu-id="22d4f-360">3</span><span class="sxs-lookup"><span data-stu-id="22d4f-360">3</span></span> |<span data-ttu-id="22d4f-361">toosave minne, LRU och minimal TTL-algoritmer är uppskattade algoritmer i stället för exakt algoritmer.</span><span class="sxs-lookup"><span data-stu-id="22d4f-361">toosave memory, LRU and minimal TTL algorithms are approximated algorithms instead of precise algorithms.</span></span> <span data-ttu-id="22d4f-362">Redis kontrollerar tre nycklar och plockningar hello en som används mindre nyligen som standard.</span><span class="sxs-lookup"><span data-stu-id="22d4f-362">By default Redis checks three keys and picks hello one that was used less recently.</span></span> |
| `lua-time-limit` |<span data-ttu-id="22d4f-363">5,000</span><span class="sxs-lookup"><span data-stu-id="22d4f-363">5,000</span></span> |<span data-ttu-id="22d4f-364">Maximal körningstid för skript Lua i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="22d4f-364">Max execution time of a Lua script in milliseconds.</span></span> <span data-ttu-id="22d4f-365">Om hello maximal körningstid uppnås loggar Redis att ett skript är fortfarande i körningen efter hello högsta tillåtna tid och startar tooreply tooqueries med ett fel.</span><span class="sxs-lookup"><span data-stu-id="22d4f-365">If hello maximum execution time is reached, Redis logs that a script is still in execution after hello maximum allowed time, and starts tooreply tooqueries with an error.</span></span> |
| `lua-event-limit` |<span data-ttu-id="22d4f-366">500</span><span class="sxs-lookup"><span data-stu-id="22d4f-366">500</span></span> |<span data-ttu-id="22d4f-367">Maxstorlek på skriptet händelsekön.</span><span class="sxs-lookup"><span data-stu-id="22d4f-367">Max size of script event queue.</span></span> |
| <span data-ttu-id="22d4f-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span><span class="sxs-lookup"><span data-stu-id="22d4f-368">`client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub`</span></span> |<span data-ttu-id="22d4f-369">0 0 032mb 8mb 60</span><span class="sxs-lookup"><span data-stu-id="22d4f-369">0 0 032mb 8mb 60</span></span> |<span data-ttu-id="22d4f-370">hello klienten utdata buffert gränser kan vara används tooforce frånkoppling av klienter som inte läsning av data från hello servern tillräckligt snabbt av någon anledning (en vanlig orsak är att en Pub/Sub-klient inte kan använda meddelanden snabb hello publisher kan ge dem).</span><span class="sxs-lookup"><span data-stu-id="22d4f-370">hello client output buffer limits can be used tooforce disconnection of clients that are not reading data from hello server fast enough for some reason (a common reason is that a Pub/Sub client can't consume messages as fast as hello publisher can produce them).</span></span> <span data-ttu-id="22d4f-371">Mer information finns i [http://redis.io/topics/clients](http://redis.io/topics/clients).</span><span class="sxs-lookup"><span data-stu-id="22d4f-371">For more information, see [http://redis.io/topics/clients](http://redis.io/topics/clients).</span></span> |

<span data-ttu-id="22d4f-372"><a name="databases"></a>
<sup>1</sup>hello gränsen för `databases` är olika för varje Azure Redis-Cache prisnivån och kan anges på Skapa cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-372"><a name="databases"></a>
<sup>1</sup>hello limit for `databases` is different for each Azure Redis Cache pricing tier and can be set at cache creation.</span></span> <span data-ttu-id="22d4f-373">Om inget `databases` inställningen anges under skapande av cache hello standardvärdet är 16.</span><span class="sxs-lookup"><span data-stu-id="22d4f-373">If no `databases` setting is specified during cache creation, hello default is 16.</span></span>

* <span data-ttu-id="22d4f-374">Basic och Standard cacheminnen</span><span class="sxs-lookup"><span data-stu-id="22d4f-374">Basic and Standard caches</span></span>
  * <span data-ttu-id="22d4f-375">C0 (250 MB) cache - av too16 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-375">C0 (250 MB) cache - up too16 databases</span></span>
  * <span data-ttu-id="22d4f-376">C1 (1 GB)-cache - av too16 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-376">C1 (1 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="22d4f-377">C2 (2,5 GB)-cache - av too16 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-377">C2 (2.5 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="22d4f-378">C3 (6 GB)-cache - av too16 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-378">C3 (6 GB) cache - up too16 databases</span></span>
  * <span data-ttu-id="22d4f-379">C4 (13 GB)-cache - av too32 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-379">C4 (13 GB) cache - up too32 databases</span></span>
  * <span data-ttu-id="22d4f-380">C5 (26 GB)-cache - av too48 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-380">C5 (26 GB) cache - up too48 databases</span></span>
  * <span data-ttu-id="22d4f-381">C6 (53 GB)-cache - av too64 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-381">C6 (53 GB) cache - up too64 databases</span></span>
* <span data-ttu-id="22d4f-382">Premium-cache</span><span class="sxs-lookup"><span data-stu-id="22d4f-382">Premium caches</span></span>
  * <span data-ttu-id="22d4f-383">P1 (6 GB - 60 GB) - upp too16 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-383">P1 (6 GB - 60 GB) - up too16 databases</span></span>
  * <span data-ttu-id="22d4f-384">P2 (13 GB - 130 GB) - upp too32 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-384">P2 (13 GB - 130 GB) - up too32 databases</span></span>
  * <span data-ttu-id="22d4f-385">P3 (26 GB - 260 GB) - upp too48 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-385">P3 (26 GB - 260 GB) - up too48 databases</span></span>
  * <span data-ttu-id="22d4f-386">P4 (53 GB - 530 GB) - upp too64 databaser</span><span class="sxs-lookup"><span data-stu-id="22d4f-386">P4 (53 GB - 530 GB) - up too64 databases</span></span>
  * <span data-ttu-id="22d4f-387">Alla premium cacheminnen med Redis cluster aktiverat – Redis cluster endast stöd för användning av databas 0 så hello `databases` begränsa för alla premium-cache med Redis cluster aktiverat effektivt är 1 och hello [Välj](http://redis.io/commands/select) kommandot är inte tillåtet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-387">All premium caches with Redis cluster enabled - Redis cluster only supports use of database 0 so hello `databases` limit for any premium cache with Redis cluster enabled is effectively 1 and hello [Select](http://redis.io/commands/select) command is not allowed.</span></span> <span data-ttu-id="22d4f-388">Mer information finns i [behöver jag toomake eventuella ändringar toomy klienten programmet toouse kluster?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span><span class="sxs-lookup"><span data-stu-id="22d4f-388">For more information, see [Do I need toomake any changes toomy client application toouse clustering?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)</span></span>

<span data-ttu-id="22d4f-389">Mer information om databaser finns [vad är Redis databaser?](cache-faq.md#what-are-redis-databases)</span><span class="sxs-lookup"><span data-stu-id="22d4f-389">For more information about databases, see [What are Redis databases?](cache-faq.md#what-are-redis-databases)</span></span>

> [!NOTE]
> <span data-ttu-id="22d4f-390">Hej `databases` inställning kan vara konfigurerade endast under Skapa cache och endast med hjälp av PowerShell, CLI eller annan av hanteringsklienter.</span><span class="sxs-lookup"><span data-stu-id="22d4f-390">hello `databases` setting can be configured only during cache creation and only using PowerShell, CLI, or other management clients.</span></span> <span data-ttu-id="22d4f-391">Ett exempel på hur du konfigurerar `databases` under Skapa cache med hjälp av PowerShell, se [ny AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span><span class="sxs-lookup"><span data-stu-id="22d4f-391">For an example of configuring `databases` during cache creation using PowerShell, see [New-AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).</span></span>
> 
> 

<span data-ttu-id="22d4f-392"><a name="maxclients"></a>
<sup>2</sup> `maxclients` är olika för varje Azure Redis-Cache prisnivån.</span><span class="sxs-lookup"><span data-stu-id="22d4f-392"><a name="maxclients"></a>
<sup>2</sup>`maxclients` is different for each Azure Redis Cache pricing tier.</span></span>

* <span data-ttu-id="22d4f-393">Basic och Standard cacheminnen</span><span class="sxs-lookup"><span data-stu-id="22d4f-393">Basic and Standard caches</span></span>
  * <span data-ttu-id="22d4f-394">C0 (250 MB) cache - too256 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-394">C0 (250 MB) cache - up too256 connections</span></span>
  * <span data-ttu-id="22d4f-395">C1 (1 GB)-cache - in too1, 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-395">C1 (1 GB) cache - up too1,000 connections</span></span>
  * <span data-ttu-id="22d4f-396">C2 (2,5 GB)-cache - in too2, 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-396">C2 (2.5 GB) cache - up too2,000 connections</span></span>
  * <span data-ttu-id="22d4f-397">C3 (6 GB)-cache - in too5, 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-397">C3 (6 GB) cache - up too5,000 connections</span></span>
  * <span data-ttu-id="22d4f-398">C4 (13 GB)-cache - in too10, 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-398">C4 (13 GB) cache - up too10,000 connections</span></span>
  * <span data-ttu-id="22d4f-399">C5 (26 GB)-cache - in too15, 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-399">C5 (26 GB) cache - up too15,000 connections</span></span>
  * <span data-ttu-id="22d4f-400">C6 (53 GB)-cache - in too20, 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-400">C6 (53 GB) cache - up too20,000 connections</span></span>
* <span data-ttu-id="22d4f-401">Premium-cache</span><span class="sxs-lookup"><span data-stu-id="22d4f-401">Premium caches</span></span>
  * <span data-ttu-id="22d4f-402">P1 (6 GB - 60 GB) - in too7, 500-anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-402">P1 (6 GB - 60 GB) - up too7,500 connections</span></span>
  * <span data-ttu-id="22d4f-403">P2 (13 GB - 130 GB) - upp too15 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-403">P2 (13 GB - 130 GB) - up too15,000 connections</span></span>
  * <span data-ttu-id="22d4f-404">P3 (26 GB - 260 GB) - upp too30 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-404">P3 (26 GB - 260 GB) - up too30,000 connections</span></span>
  * <span data-ttu-id="22d4f-405">P4 (53 GB - 530 GB) - upp too40 000 anslutningar</span><span class="sxs-lookup"><span data-stu-id="22d4f-405">P4 (53 GB - 530 GB) - up too40,000 connections</span></span>

> [!NOTE]
> <span data-ttu-id="22d4f-406">Med varje cachens storlek kan *upp till* ett visst antal anslutningar, varje anslutning tooRedis har associeras med den.</span><span class="sxs-lookup"><span data-stu-id="22d4f-406">While each size of cache allows *up to* a certain number of connections, each connection tooRedis has overhead associated with it.</span></span> <span data-ttu-id="22d4f-407">Ett exempel på sådana kostnader är processor- och minnesanvändning på grund av TLS/SSL-kryptering.</span><span class="sxs-lookup"><span data-stu-id="22d4f-407">An example of such overhead would be CPU and memory usage as a result of TLS/SSL encryption.</span></span> <span data-ttu-id="22d4f-408">hello anslutningens maximala gränsen för en given cachestorlek förutsätter en lågt belastade cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-408">hello maximum connection limit for a given cache size assumes a lightly loaded cache.</span></span> <span data-ttu-id="22d4f-409">Om att läsa in från anslutningen kostnader *plus* belastningen från Klientåtgärder överskrider kapaciteten för hello system kan hello cache kan ha problem med kapacitet även om du inte har överskridit hello anslutningsgräns hello aktuella cachestorleken.</span><span class="sxs-lookup"><span data-stu-id="22d4f-409">If load from connection overhead *plus* load from client operations exceeds capacity for hello system, hello cache can experience capacity issues even if you have not exceeded hello connection limit for hello current cache size.</span></span>
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a><span data-ttu-id="22d4f-410">Redis-kommandon som inte stöds i Azure Redis-Cache</span><span class="sxs-lookup"><span data-stu-id="22d4f-410">Redis commands not supported in Azure Redis Cache</span></span>
> [!IMPORTANT]
> <span data-ttu-id="22d4f-411">Eftersom konfiguration och hantering av Azure Redis-Cache instanser hanteras av Microsoft, har hello följande kommandon inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="22d4f-411">Because configuration and management of Azure Redis Cache instances is managed by Microsoft, hello following commands are disabled.</span></span> <span data-ttu-id="22d4f-412">Om du försöker tooinvoke dem du får ett felmeddelande för`"(error) ERR unknown command"`.</span><span class="sxs-lookup"><span data-stu-id="22d4f-412">If you try tooinvoke them, you receive an error message similar too`"(error) ERR unknown command"`.</span></span>
> 
> * <span data-ttu-id="22d4f-413">BGREWRITEAOF</span><span class="sxs-lookup"><span data-stu-id="22d4f-413">BGREWRITEAOF</span></span>
> * <span data-ttu-id="22d4f-414">BGSAVE</span><span class="sxs-lookup"><span data-stu-id="22d4f-414">BGSAVE</span></span>
> * <span data-ttu-id="22d4f-415">CONFIG</span><span class="sxs-lookup"><span data-stu-id="22d4f-415">CONFIG</span></span>
> * <span data-ttu-id="22d4f-416">FELSÖKA</span><span class="sxs-lookup"><span data-stu-id="22d4f-416">DEBUG</span></span>
> * <span data-ttu-id="22d4f-417">MIGRERA</span><span class="sxs-lookup"><span data-stu-id="22d4f-417">MIGRATE</span></span>
> * <span data-ttu-id="22d4f-418">SPARA</span><span class="sxs-lookup"><span data-stu-id="22d4f-418">SAVE</span></span>
> * <span data-ttu-id="22d4f-419">AVSTÄNGNING</span><span class="sxs-lookup"><span data-stu-id="22d4f-419">SHUTDOWN</span></span>
> * <span data-ttu-id="22d4f-420">SLAVEOF</span><span class="sxs-lookup"><span data-stu-id="22d4f-420">SLAVEOF</span></span>
> * <span data-ttu-id="22d4f-421">KLUSTER - kluster skriva kommandon inaktiveras, men skrivskyddad klustret kommandon tillåts.</span><span class="sxs-lookup"><span data-stu-id="22d4f-421">CLUSTER - Cluster write commands are disabled, but read-only Cluster commands are permitted.</span></span>
> 
> 

<span data-ttu-id="22d4f-422">Mer information om Redis-kommandon finns [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="22d4f-422">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>

## <a name="redis-console"></a><span data-ttu-id="22d4f-423">Redis-konsolen</span><span class="sxs-lookup"><span data-stu-id="22d4f-423">Redis console</span></span>
<span data-ttu-id="22d4f-424">Du kan på ett säkert sätt skicka kommandon tooyour Azure Redis-Cache-instanser som använder hello **Redis-konsolen**, som finns i hello Azure portal för alla nivåer i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-424">You can securely issue commands tooyour Azure Redis Cache instances using hello **Redis Console**, which is available in hello Azure portal for all cache tiers.</span></span>

> [!IMPORTANT]
> - <span data-ttu-id="22d4f-425">Hej Redis-konsolen fungerar inte med [VNET](cache-how-to-premium-vnet.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-425">hello Redis Console does not work with [VNET](cache-how-to-premium-vnet.md).</span></span> <span data-ttu-id="22d4f-426">När din cache är en del av ett VNET, endast klienter i hello VNET kan komma åt hello cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-426">When your cache is part of a VNET, only clients in hello VNET can access hello cache.</span></span> <span data-ttu-id="22d4f-427">Eftersom Redis-konsolen körs i din lokala webbläsare som är utanför hello VNET, kan den inte kan ansluta tooyour cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-427">Because Redis Console runs in your local browser, which is outside hello VNET, it can't connect tooyour cache.</span></span>
> - <span data-ttu-id="22d4f-428">Inte alla Redis-kommandon stöds i Azure Redis-Cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-428">Not all Redis commands are supported in Azure Redis Cache.</span></span> <span data-ttu-id="22d4f-429">En lista över Redis-kommandon som är inaktiverat för Azure Redis-Cache, finns i föregående hello [Redis-kommandon som inte stöds i Azure Redis-Cache](#redis-commands-not-supported-in-azure-redis-cache) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="22d4f-429">For a list of Redis commands that are disabled for Azure Redis Cache, see hello previous [Redis commands not supported in Azure Redis Cache](#redis-commands-not-supported-in-azure-redis-cache) section.</span></span> <span data-ttu-id="22d4f-430">Mer information om Redis-kommandon finns [http://redis.io/commands](http://redis.io/commands).</span><span class="sxs-lookup"><span data-stu-id="22d4f-430">For more information about Redis commands, see [http://redis.io/commands](http://redis.io/commands).</span></span>
> 
> 

<span data-ttu-id="22d4f-431">tooaccess hello Redis-konsolen klickar du på **konsolen** från hello **Redis-Cache** bladet.</span><span class="sxs-lookup"><span data-stu-id="22d4f-431">tooaccess hello Redis Console, click **Console** from hello **Redis Cache** blade.</span></span>

![Redis-konsolen](./media/cache-configure/redis-console-menu.png)

<span data-ttu-id="22d4f-433">tooissue kommandon mot din cache-instans, bara typen hello önskade kommandot i hello-konsolen.</span><span class="sxs-lookup"><span data-stu-id="22d4f-433">tooissue commands against your cache instance, simply type hello desired command into hello console.</span></span>

![Redis-konsolen](./media/cache-configure/redis-console.png)


### <a name="using-hello-redis-console-with-a-premium-clustered-cache"></a><span data-ttu-id="22d4f-435">Med hjälp av hello cache Redis-konsolen med en premium för</span><span class="sxs-lookup"><span data-stu-id="22d4f-435">Using hello Redis Console with a premium clustered cache</span></span>

<span data-ttu-id="22d4f-436">När cache med en premium hello Redis-konsolen för, kan du utfärda kommandon tooa enda Fragmentera hello-cache.</span><span class="sxs-lookup"><span data-stu-id="22d4f-436">When using hello Redis Console with a premium clustered cache, you can issue commands tooa single shard of hello cache.</span></span> <span data-ttu-id="22d4f-437">tooissue ett kommando tooa specifika Fragmentera, först ansluta toohello önskade Fragmentera genom att klicka på hello Fragmentera väljare.</span><span class="sxs-lookup"><span data-stu-id="22d4f-437">tooissue a command tooa specific shard, first connect toohello desired shard by clicking it on hello shard picker.</span></span>

![Redis-konsolen](./media/cache-configure/redis-console-premium-cluster.png)

<span data-ttu-id="22d4f-439">Om du försöker tooaccess en nyckel som lagras i en annan Fragmentera än hello anslutna Fragmentera får du ett fel meddelande liknande toohello efter meddelande.</span><span class="sxs-lookup"><span data-stu-id="22d4f-439">If you attempt tooaccess a key that is stored in a different shard than hello connected shard, you receive an error message similar toohello following message.</span></span>

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

<span data-ttu-id="22d4f-440">I föregående exempel hello Fragmentera 1 är hello valda Fragmentera, men `myKey` finns i Fragmentera 0, som anges av hello `(shard 0)` del av hello felmeddelande.</span><span class="sxs-lookup"><span data-stu-id="22d4f-440">In hello previous example, shard 1 is hello selected shard, but `myKey` is located in shard 0, as indicated by hello `(shard 0)` portion of hello error message.</span></span> <span data-ttu-id="22d4f-441">I det här exemplet tooaccess `myKey`väljer Fragmentera 0 med hello Fragmentera Väljaren och problemet hello önskad kommando.</span><span class="sxs-lookup"><span data-stu-id="22d4f-441">In this example, tooaccess `myKey`, select shard 0 using hello shard picker, and then issue hello desired command.</span></span>


## <a name="move-your-cache-tooa-new-subscription"></a><span data-ttu-id="22d4f-442">Flytta den nya prenumerationen för cache-tooa</span><span class="sxs-lookup"><span data-stu-id="22d4f-442">Move your cache tooa new subscription</span></span>
<span data-ttu-id="22d4f-443">Du kan flytta den nya prenumerationen för cache-tooa genom att klicka på **flytta**.</span><span class="sxs-lookup"><span data-stu-id="22d4f-443">You can move your cache tooa new subscription by clicking **Move**.</span></span>

![Flytta Redis-Cache](./media/cache-configure/redis-cache-move.png)

<span data-ttu-id="22d4f-445">Information om hur du flyttar resurser från en resurs grupp tooanother och från en prenumeration tooanother finns [flytta resurser toonew resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="22d4f-445">For information on moving resources from one resource group tooanother, and from one subscription tooanother, see [Move resources toonew resource group or subscription](../azure-resource-manager/resource-group-move-resources.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="22d4f-446">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="22d4f-446">Next steps</span></span>
* <span data-ttu-id="22d4f-447">Mer information om hur du arbetar med Redis-kommandon finns [hur kan jag köra Redis kommandon?](cache-faq.md#how-can-i-run-redis-commands)</span><span class="sxs-lookup"><span data-stu-id="22d4f-447">For more information on working with Redis commands, see [How can I run Redis commands?](cache-faq.md#how-can-i-run-redis-commands)</span></span>

