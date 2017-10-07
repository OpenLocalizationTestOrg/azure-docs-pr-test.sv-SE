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
# <a name="how-tooconfigure-azure-redis-cache"></a>Hur tooconfigure Azure Redis-Cache
Det här avsnittet beskrivs hur tooreview och uppdatera hello-konfiguration för Azure Redis-Cache-instanser och omfattar hello Redis server standardkonfigurationen för Azure Redis-Cache-instanser.

> [!NOTE]
> Mer information om hur du konfigurerar och använder premium cache-funktioner finns [hur tooconfigure beständiga](cache-how-to-premium-persistence.md), [hur tooconfigure clustering](cache-how-to-premium-clustering.md), och [hur stöder tooconfigure virtuellt nätverk ](cache-how-to-premium-vnet.md).
> 
> 

## <a name="configure-redis-cache-settings"></a>Konfigurera inställningar för Redis-cache
[!INCLUDE [redis-cache-create](../../includes/redis-cache-browse.md)]

Azure Redis-Cache-inställningar visas och konfigureras på hello **Redis-Cache** blad med hjälp av hello **resurs menyn**.

![Redis-Cache-inställningar](./media/cache-configure/redis-cache-settings.png)

Du kan visa och konfigurera följande inställningar med hjälp av hello hello **resurs menyn**.

* [Översikt](#overview)
* [Aktivitetslogg](#activity-log)
* [Åtkomstkontroll (IAM)](#access-control-iam)
* [Taggar](#tags)
* [Diagnostisera och lösa problem](#diagnose-and-solve-problems)
* [Inställningar](#settings)
    * [Snabbtangenter](#access-keys)
    * [Avancerade inställningar](#advanced-settings)
    * [Redis-Cache Advisor](#redis-cache-advisor)
    * [Skalning](#scale)
    * [Redis klusterstorleken](#cluster-size)
    * [Redis-datapersistence](#redis-data-persistence)
    * [Schemauppdateringar](#schedule-updates)
    * [Geo-replikering](#geo-replication)
    * [Virtual Network](#virtual-network)
    * [Brandvägg](#firewall)
    * [Egenskaper](#properties)
    * [Lås](#locks)
    * [Automation-skript](#automation-script)
* [Administration](#administration)
    * [Importera data](#importexport)
    * [Exportera data](#importexport)
    * [Starta om](#reboot)
* [Övervakning](#monitoring)
    * [Redis-mått](#redis-metrics)
    * [Aviseringsregler](#alert-rules)
    * [Diagnostik](#diagnostics)
* [Support och felsökning av inställningar](#support-amp-troubleshooting-settings)
    * [Resurshälsa](#resource-health)
    * [Ny supportbegäran](#new-support-request)


## <a name="overview"></a>Översikt

**Översikt över** ger dig med grundläggande information om ditt cacheminne, till exempel namn, portar, prisnivå, och valda cache mått.

### <a name="activity-log"></a>Aktivitetslogg

Klicka på **aktivitetsloggen** tooview åtgärder som utförs på ditt cacheminne. Du kan också använda filtrering tooexpand den här vyn tooinclude andra resurser. Mer information om hur du arbetar med granskningsloggarna finns [granskningsåtgärder med Resource Manager](../azure-resource-manager/resource-group-audit.md). Mer information om hur du övervakar Azure Redis-Cache-händelser finns [åtgärder och aviseringar](cache-how-to-monitor.md#operations-and-alerts).

### <a name="access-control-iam"></a>Åtkomstkontroll (IAM)

Hej **åtkomstkontroll (IAM)** avsnittet ger stöd för rollbaserad åtkomstkontroll (RBAC) i hello Azure portal toohelp organisationer uppfyller deras åtkomst hanteringskrav bara och exakt. Mer information finns i [rollbaserad åtkomstkontroll i hello Azure-portalen](../active-directory/role-based-access-control-configure.md).

### <a name="tags"></a>Taggar

Hej **taggar** avsnitt kan du ordna dina resurser. Mer information finns i [med hjälp av taggar tooorganize resurserna i Azure](../azure-resource-manager/resource-group-using-tags.md).


### <a name="diagnose-and-solve-problems"></a>Diagnosticera och lösa problem

Klicka på **diagnostisera och lösa problem** toobe medföljer vanliga problem och strategier för att lösa dem.



## <a name="settings"></a>Inställningar
Hej **inställningar** avsnittet tillåter tooaccess och konfigurera följande inställningar för ditt cacheminne hello.

* [Snabbtangenter](#access-keys)
* [Avancerade inställningar](#advanced-settings)
* [Redis-Cache Advisor](#redis-cache-advisor)
* [Skalning](#scale)
* [Redis klusterstorleken](#cluster-size)
* [Redis-datapersistence](#redis-data-persistence)
* [Schemauppdateringar](#schedule-updates)
* [Geo-replikering](#geo-replication)
* [Virtual Network](#virtual-network)
* [Brandvägg](#firewall)
* [Egenskaper](#properties)
* [Lås](#locks)
* [Automation-skript](#automation-script)



### <a name="access-keys"></a>Åtkomstnycklar
Klicka på **åtkomstnycklar** tooview eller generera hello åtkomstnycklar för ditt cacheminne. De här nycklarna som används av hello-klienter som ansluter tooyour cache.

![Redis-Cache snabbtangenter](./media/cache-configure/redis-cache-manage-keys.png)

### <a name="advanced-settings"></a>Avancerade inställningar
hello följande inställningar konfigureras på hello **avancerade inställningar** bladet.

* [Åtkomstportar](#access-ports)
* [Principer för minne](#memory-policies)
* [Keyspace-meddelanden (avancerade inställningar)](#keyspace-notifications-advanced-settings)

#### <a name="access-ports"></a>Åtkomstportar
Som standard är icke-SSL-åtkomst inaktiverad för nya cacheminnen. tooenable hello icke-SSL-port, klickar du på **nr** för **Tillåt åtkomst endast via SSL** på hello **avancerade inställningar** bladet och klicka sedan på **spara**.

![Åtkomstportar för redis-Cache](./media/cache-configure/redis-cache-access-ports.png)

<a name="maxmemory-policy-and-maxmemory-reserved"></a>
#### <a name="memory-policies"></a>Principer för minne
Hej **Maxmemory princip**, **maxmemory-reserverade**, och **maxfragmentationmemory-reserverade** inställningar på hello **avancerade inställningar**bladet konfigurera hello minne principer för hello-cachen.

![Redis-Cache Maxmemory princip](./media/cache-configure/redis-cache-maxmemory-policy.png)

**Maxmemory princip** konfigurerar hello av principen för hello cache och låter dig toochoose från hello efter borttagning principer:

* `volatile-lru`-Detta är hello standard.
* `allkeys-lru`
* `volatile-random`
* `allkeys-random`
* `volatile-ttl`
* `noeviction`

Mer information om `maxmemory` principer, se [av principer](http://redis.io/topics/lru-cache#eviction-policies).

Hej **maxmemory-reserverade** inställningen konfigurerar hello mängden minne i MB som har reserverats för icke-cache-åtgärder som replikering under växling vid fel. Det här värdet kan du toohave en mer konsekvent användarupplevelse för Redis-servern när din belastningen varierar. Det här värdet ska anges för arbetsbelastningar som är aktiverat. När minnet reserveras för dessa åtgärder är inte tillgänglig för lagring av cachelagrade data.

Hej **maxfragmentationmemory-reserverade** inställningen konfigurerar hello mängden minne i MB som är reserverade tooaccommodate för minnesfragmentering. Det här värdet kan du toohave en mer konsekvent användarupplevelse för Redis-servern när hello-cachen är full eller Stäng toofull och hello fragmentering förhållandet också är hög. När minnet reserveras för dessa åtgärder är inte tillgänglig för lagring av cachelagrade data.

En sak tooconsider när du väljer ett nytt minne reservation värde (**maxmemory-reserverade** eller **maxfragmentationmemory-reserverade**) är hur den här ändringen kan påverka en cache som redan körs med stora mängder data. Om du har en 53 GB cache med 49 GB data och sedan ändra hello reservation värdet too8 GB, till exempel kommer detta släppa Hej max tillgängligt minne för hello ned too45 GB. Om din aktuella `used_memory` eller `used_memory_rss` värden är högre än hello ny gräns 45 GB och sedan hello systemet har tooevict data förrän både `used_memory` och `used_memory_rss` är lägre än 45 GB. Borttagning kan öka belastningen och minne fragmentering på servern. Mer information om cache mått som `used_memory` och `used_memory_rss`, se [tillgängliga mått och rapportering intervall](cache-how-to-monitor.md#available-metrics-and-reporting-intervals).

> [!IMPORTANT]
> Hej **maxmemory-reserverade** och **maxfragmentationmemory-reserverade** inställningarna är bara tillgängliga för Standard och Premium cachelagrar.
> 
> 

#### <a name="keyspace-notifications-advanced-settings"></a>Keyspace-meddelanden (avancerade inställningar)
Redis keyspace-meddelanden är konfigurerade på hello **avancerade inställningar** bladet. Keyspace-meddelanden tillåter klienter tooreceive meddelanden när vissa händelser inträffar.

![Redis-Cache avancerade inställningar](./media/cache-configure/redis-cache-advanced-settings.png)

> [!IMPORTANT]
> Keyspace-meddelanden och hello **meddela-keyspace-händelser** inställningen är bara tillgängliga för Standard- och Premium.
> 
> 

Mer information finns i [Redis Keyspace-meddelanden](http://redis.io/topics/notifications). Exempelkod finns hello [KeySpaceNotifications.cs](https://github.com/rustd/RedisSamples/blob/master/HelloWorld/KeySpaceNotifications.cs) filen i hello [Hello world](https://github.com/rustd/RedisSamples/tree/master/HelloWorld) exempel.


<a name="recommendations"></a>
## <a name="redis-cache-advisor"></a>Redis-Cache Advisor
Hej **Redis-Cache Advisor** bladet visar rekommendationer för ditt cacheminne. Inga rekommendationer visas under normal drift. 

![Rekommendationer](./media/cache-configure/redis-cache-no-recommendations.png)

Om alla villkor som uppstår under hello drift av ditt cacheminne som hög minnesanvändning, nätverkets bandbredd eller belastningen på servern, visas en avisering på hello **Redis-Cache** bladet.

![Rekommendationer](./media/cache-configure/redis-cache-recommendations-alert.png)

Mer information finns på hello **rekommendationer** bladet.

![Rekommendationer](./media/cache-configure/redis-cache-recommendations.png)

Du kan övervaka de här måtten på hello [övervakning diagram](cache-how-to-monitor.md#monitoring-charts) och [användning diagram](cache-how-to-monitor.md#usage-charts) avsnitt i hello **Redis-Cache** bladet.

Varje prisnivå har olika begränsningar för klientanslutningar, minne och bandbredd. Om ditt cacheminne närmar sig maximal kapacitet för de här måtten över en längre period, skapas en rekommendation. Mer information om hello mått och gränser som granskas av hello **rekommendationer** verktyget, se följande tabell hello:

| Redis-Cache-mått | Mer information |
| --- | --- |
| Användningen av nätverksbandbredd |[Prestanda för cache - bandbredd](cache-faq.md#cache-performance) |
| Anslutna klienter |[Server för standardkonfigurationen - Redis-maxclients](#maxclients) |
| Belastningen på servern |[Användning-diagram - Redis-serverbelastning](cache-how-to-monitor.md#usage-charts) |
| Minnesanvändning |[Prestanda för cache - storleken](cache-faq.md#cache-performance) |

tooupgrade ditt cacheminne, klickar du på **uppgradera nu** toochange hello prisnivån och [skala](#scale) ditt cacheminne. Mer information om hur du väljer en prisnivå finns [vilka Redis-Cache erbjudande och vilken storlek ska jag använda?](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)


### <a name="scale"></a>Skala
Klicka på **skala** tooview eller ändra hello prisnivån för din cache. Läs mer om att skala [hur tooScale Azure Redis-Cache](cache-how-to-scale.md).

![Redis-Cache prisnivån](./media/cache-configure/pricing-tier.png)

<a name="cluster-size"></a>

### <a name="redis-cluster-size"></a>Redis klusterstorleken
Klicka på **(FÖRHANDSGRANSKNING) Redis klusterstorleken** toochange hello klusterstorleken körs bidrag cachelagra med aktiverad klustring.

> [!NOTE]
> Observera att när hello Azure Redis Cache Premium nivå har publicerat tooGeneral tillgänglighet, hello Redis klusterstorleken funktionen är för närvarande under förhandsgranskning.
> 
> 

![Redis klusterstorleken](./media/cache-configure/redis-cache-redis-cluster-size.png)

klusterstorleken för toochange hello hello skjutreglaget eller ange ett tal mellan 1 och 10 i hello **Fragmentera antal** textrutan och klicka på **OK** toosave.

> [!IMPORTANT]
> Redis-kluster är endast tillgängligt för Premium. Mer information finns i [hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md).
> 
> 


### <a name="redis-data-persistence"></a>Redis-datapersistence
Klicka på **Redis-datapersistence** tooenable, inaktivera eller konfigurera datapersistence för premium-cache. Azure Redis-Cache erbjuder Redis-datapersistence med antingen [RDB beständiga](cache-how-to-premium-persistence.md#configure-rdb-persistence) eller [AOF beständiga](cache-how-to-premium-persistence.md#configure-aof-persistence).

Mer information finns i [hur tooconfigure persistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md).


> [!IMPORTANT]
> Redis-datapersistence är endast tillgängligt för Premium. 
> 
> 

### <a name="schedule-updates"></a>Schemauppdateringar
Hej **schemalägga uppdateringar** bladet kan du toodesignate en underhållsperiod för Redis-serveruppdateringar för ditt cacheminne. 

> [!IMPORTANT]
> Hej underhållsperioden gäller endast uppdateringar för tooRedis server och inte tooany Azure uppdaterar eller uppdaterar toohello operativsystemet hello virtuella datorer som är värdar för hello cache.
> 
> 

![Schemauppdateringar](./media/cache-configure/redis-schedule-updates.png)

toospecify en underhållsperiod hello önskad dagar och ange hello fönstret starttid för underhåll för varje dag och klickar på **OK**. Observera att hello underhållsfönstertiden i UTC. 

> [!IMPORTANT]
> Hej **schemalägga uppdateringar** funktioner är endast tillgängligt för Premium-nivån. Mer information och instruktioner finns i [Azure Redis-Cache administration - Schemalägg uppdateringar](cache-administration.md#schedule-updates).
> 
> 

### <a name="geo-replication"></a>Geo-replikering

Hej **georeplikering** bladet tillhandahåller en mekanism för att länka två instanser i Premium-nivån Azure Redis-Cache. En cache utses hello primära länkade cache och hello andra som sekundär länkade hello-cache. hello sekundär länkade cache skrivskyddas och data som skrivits toohello primära cache är replikeras toohello sekundära länkade cache. Den här funktionen kan vara används tooreplicate en cache i Azure-regioner.

> [!IMPORTANT]
> **GEO-replikering** är endast tillgängligt för Premium-nivån. Mer information och instruktioner finns i [hur tooconfigure Geo-replikering för Azure Redis-Cache](cache-how-to-geo-replication.md).
> 
> 

### <a name="virtual-network"></a>Virtual Network
Hej **virtuellt nätverk** avsnittet kan du tooconfigure hello virtuella nätverksinställningar för ditt cacheminne. Information om hur du skapar en premium-cache med VNET stöd och uppdaterar sina inställningar Se [hur tooconfigure för virtuella nätverk som har stöd för Premium Azure Redis-Cache](cache-how-to-premium-vnet.md).

> [!IMPORTANT]
> Inställningarna för virtuella nätverk är bara tillgängliga för premium som har konfigurerats med stöd för virtuellt nätverk under skapande av cachen. 
> 
> 

### <a name="firewall"></a>Brandvägg

Klicka på **brandväggen** tooview och konfigurera brandväggsregler för din Premium Azure Redis-Cache.

![Brandvägg](./media/cache-configure/redis-firewall-rules.png)

Du kan ange brandväggsregler med en start- och IP-adressintervall. När brandväggsregler har konfigurerats, ange bara klientanslutningar från hello IP-adressintervall kan ansluta toohello cache. När en brandväggsregel sparas finns det en kort fördröjning innan hello regel börjar gälla. Den här fördröjningen är vanligtvis mindre än en minut.

> [!IMPORTANT]
> Anslutningar från Azure Redis-Cache övervakningssystem tillåts alltid, även om brandväggens regler är konfigurerade.
> 
> Brandväggsregler är bara tillgängliga för Premium-nivån.
> 
> 

### <a name="properties"></a>Egenskaper
Klicka på **egenskaper** tooview information om ditt cacheminne, inklusive hello cache-slutpunkten och portar.

![Redis-Cache-egenskaper](./media/cache-configure/redis-cache-properties.png)

### <a name="locks"></a>Lås
Hej **låser** avsnittet kan du toolock en prenumeration, resursgrupp eller resurs tooprevent andra användare i din organisation av misstag tas bort eller ändra viktiga resurser. Mer information finns i [Låsa resurser med Azure Resource Manager](../azure-resource-manager/resource-group-lock-resources.md).

### <a name="automation-script"></a>Automation-skript

Klicka på **automatiseringsskriptet** toobuild och exportera en mall för dina distribuerade resurser för framtida distributioner. Mer information om hur du arbetar med mallar finns [distribuera resurser med Azure Resource Manager-mallar](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="administration-settings"></a>Inställningar för administration
Hej inställningar i hello **Administration** avsnittet tillåter tooperform hello följande administrativa uppgifter för ditt cacheminne. 

![Administration](./media/cache-configure/redis-cache-administration.png)

* [Importera data](#importexport)
* [Exportera data](#importexport)
* [Starta om](#reboot)


### <a name="importexport"></a>Import/Export
Importera och exportera är en Azure Redis-Cache data management-åtgärd, som gör att du tooimport och exportera data i hello-cachen genom att importera och exportera en ögonblicksbild för Redis-Cache databasen (RDB) från en premium cache tooa sidblob i ett Azure Storage-konto. Import/Export kan du toomigrate mellan olika instanser av Azure Redis-Cache eller fylla hello cachen med data innan de används.

Importera kan vara används toobring Redis kompatibel RDB filer från en Redis-server som kör i molnet eller miljö, inklusive Redis som körs på Linux, Windows eller valfri provider som molntjänster, till exempel Amazon Web Services och andra. Importera data är ett enkelt sätt toocreate en cache med data i förväg. Azure Redis-Cache under hello importen läser in hello RDB filer från Azure storage i minnet och infogar hello nycklar i hello cache.

Exportera kan tooexport hello data som lagras i Azure Redis-Cache tooRedis kompatibel RDB filer. Du kan använda den här funktionen toomove data från en Azure Redis-Cache-instansen tooanother eller tooanother Redis-servern. Under exportprocessen hello skapas en temporär fil på hello VM att värdar hello Azure Redis-Cache server-instansen och hello fil överförda toohello avses storage-konto. När hello exporten har slutförts med antingen en status för lyckad eller misslyckad tas hello tillfälliga filen bort.

> [!IMPORTANT]
> Import/Export är endast tillgängligt för Premium-nivån. Mer information och instruktioner finns i [importera och exportera data i Azure Redis-Cache](cache-how-to-import-export-data.md).
> 
> 

### <a name="reboot"></a>Starta om
Hej **omstart** bladet kan du tooreboot hello noder i ditt cacheminne. Den här funktionen för omstart kan du tootest ditt program för återhämtning om det finns ett fel i en cachenod.

![Starta om](./media/cache-configure/redis-cache-reboot.png)

Om du har en premium-cache med aktiverad klustring, kan du välja vilka delar av hello cache tooreboot.

![Starta om](./media/cache-configure/redis-cache-reboot-cluster.png)

tooreboot en eller flera noder i ditt cacheminne, Välj önskad hello noder och klicka på **omstart**. Om du har en premium-cache med aktiverad klustring, Välj hello shard(s) tooreboot och klicka sedan på **omstart**. Hej valda noder omstart efter några minuter och är tillbaka online och ett par minuter senare.

> [!IMPORTANT]
> Starta om datorn är nu tillgänglig för alla prisnivåer. Mer information och instruktioner finns i [Azure Redis-Cache administration - omstart](cache-administration.md#reboot).
> 
> 


## <a name="monitoring"></a>Övervakning

Hej **övervakning** avsnittet kan du tooconfigure diagnostik- och övervakning för Redis-Cache. Mer information om övervakning av Azure Redis-Cache och diagnostik finns [hur toomonitor Azure Redis-Cache](cache-how-to-monitor.md).

![Diagnostik](./media/cache-configure/redis-cache-diagnostics.png)

* [Redis-mått](#redis-metrics)
* [Aviseringsregler](#alert-rules)
* [Diagnostik](#diagnostics)

### <a name="redis-metrics"></a>Redis-mått
Klicka på **Redis mått** för[visa mått](cache-how-to-monitor.md#view-cache-metrics) för ditt cacheminne.

### <a name="alert-rules"></a>Aviseringsregler

Klicka på **Varna regler** tooconfigure aviseringar baserat på Redis-Cache mått. Mer information finns i [aviseringar](cache-how-to-monitor.md#alerts).

### <a name="diagnostics"></a>Diagnostik

Som standard cache i Azure-Monitor är [lagras i 30 dagar](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) och sedan tas bort. toopersist din cache-mätvärden för längre än 30 dagar, klicka på **diagnostik** för[konfigurera hello lagringskonto](cache-how-to-monitor.md#export-cache-metrics) används toostore cache diagnostik.

>[!NOTE]
>I tillägg tooarchiving din mått toostorage cache kan du också [strömma dem tooan Event hub eller skicka dem tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

## <a name="support--troubleshooting-settings"></a>Support och felsökning av inställningar
Hej inställningar i hello **stöd + felsökning** avsnittet förse dig med alternativ för att lösa problem med ditt cacheminne.

![Stöd för + felsökning](./media/cache-configure/redis-cache-support-troubleshooting.png)

* [Resurshälsa](#resource-health)
* [Ny supportbegäran](#new-support-request)

### <a name="resource-health"></a>Resurshälsa
**Resurshälsa** bevakar resurs och anger om den körs som förväntat. Läs mer om hello Azure-resurshanteraren hälsotjänsten [Azure Resource health översikt](../resource-health/resource-health-overview.md).

> [!NOTE]
> Resurshälsotillståndet är för närvarande inte tooreport på hello hälsotillstånd Azure Redis-Cache-instanser som finns i ett virtuellt nätverk. Mer information finns i [fungerar alla cachefunktioner när värd en cachelagring i ett virtuellt nätverk?](cache-how-to-premium-vnet.md#do-all-cache-features-work-when-hosting-a-cache-in-a-vnet)
> 
> 

### <a name="new-support-request"></a>Ny supportbegäran
Klicka på **ny supportbegäran** tooopen en supportförfrågan för ditt cacheminne.





## <a name="default-redis-server-configuration"></a>Standardkonfigurationen för Redis-server
Nya Azure Redis-Cache-instanserna är konfigurerade med hello följande standardvärden för Redis-konfigurering.

> [!NOTE]
> hello inställningar i det här avsnittet kan inte ändras med hjälp av hello `StackExchange.Redis.IServer.ConfigSet` metod. Om den här metoden anropas med en hello kommandona i det här avsnittet, genereras ett undantag liknande toohello följande:  
> 
> `StackExchange.Redis.RedisServerException: ERR unknown command 'CONFIG'`
> 
> Alla värden som kan konfigureras som **maximalt minne princip**, kan konfigureras via hello Azure-portalen eller kommandoradsverktyget hanteringsverktyg, till exempel Azure CLI eller PowerShell.
> 
> 

| Inställning | Standardvärde | Beskrivning |
| --- | --- | --- |
| `databases` |16 |hello standardantalet databaser är 16 men du kan konfigurera ett annat nummer baserat på hello som prisnivå. <sup>1</sup> hello standarddatabasen är DB 0 kan du välja ett annat namn på en per anslutning grunden med hjälp av `connection.GetDatabase(dbid)` där `dbid` är ett tal mellan `0` och `databases - 1`. |
| `maxclients` |Beror på hello prisnivån<sup>2</sup> |Detta är hello högsta antal anslutna klienter tillåts vid hello samma tid. Hello gränsen har nåtts stängs Redis när alla hello nya anslutningar, returneras ett fel om 'max antal klienter som nått'. |
| `maxmemory-policy` |`volatile-lru` |Princip för Maxmemory är hello inställning för hur Redis väljer vilka tooremove när `maxmemory` (hello storlek hello cache erbjuder du valde när du skapade hello cache) har nåtts. Med Azure Redis-Cache hello standard är inställningen `volatile-lru`, som tar bort hello nycklar med ett förfallodatum som anges med en LRU-algoritm. Den här inställningen kan konfigureras i hello Azure-portalen. Mer information finns i [minne principer](#memory-policies). |
| `maxmemory-samples` |3 |toosave minne, LRU och minimal TTL-algoritmer är uppskattade algoritmer i stället för exakt algoritmer. Redis kontrollerar tre nycklar och plockningar hello en som används mindre nyligen som standard. |
| `lua-time-limit` |5,000 |Maximal körningstid för skript Lua i millisekunder. Om hello maximal körningstid uppnås loggar Redis att ett skript är fortfarande i körningen efter hello högsta tillåtna tid och startar tooreply tooqueries med ett fel. |
| `lua-event-limit` |500 |Maxstorlek på skriptet händelsekön. |
| `client-output-buffer-limit` `normalclient-output-buffer-limit` `pubsub` |0 0 032mb 8mb 60 |hello klienten utdata buffert gränser kan vara används tooforce frånkoppling av klienter som inte läsning av data från hello servern tillräckligt snabbt av någon anledning (en vanlig orsak är att en Pub/Sub-klient inte kan använda meddelanden snabb hello publisher kan ge dem). Mer information finns i [http://redis.io/topics/clients](http://redis.io/topics/clients). |

<a name="databases"></a>
<sup>1</sup>hello gränsen för `databases` är olika för varje Azure Redis-Cache prisnivån och kan anges på Skapa cache. Om inget `databases` inställningen anges under skapande av cache hello standardvärdet är 16.

* Basic och Standard cacheminnen
  * C0 (250 MB) cache - av too16 databaser
  * C1 (1 GB)-cache - av too16 databaser
  * C2 (2,5 GB)-cache - av too16 databaser
  * C3 (6 GB)-cache - av too16 databaser
  * C4 (13 GB)-cache - av too32 databaser
  * C5 (26 GB)-cache - av too48 databaser
  * C6 (53 GB)-cache - av too64 databaser
* Premium-cache
  * P1 (6 GB - 60 GB) - upp too16 databaser
  * P2 (13 GB - 130 GB) - upp too32 databaser
  * P3 (26 GB - 260 GB) - upp too48 databaser
  * P4 (53 GB - 530 GB) - upp too64 databaser
  * Alla premium cacheminnen med Redis cluster aktiverat – Redis cluster endast stöd för användning av databas 0 så hello `databases` begränsa för alla premium-cache med Redis cluster aktiverat effektivt är 1 och hello [Välj](http://redis.io/commands/select) kommandot är inte tillåtet. Mer information finns i [behöver jag toomake eventuella ändringar toomy klienten programmet toouse kluster?](cache-how-to-premium-clustering.md#do-i-need-to-make-any-changes-to-my-client-application-to-use-clustering)

Mer information om databaser finns [vad är Redis databaser?](cache-faq.md#what-are-redis-databases)

> [!NOTE]
> Hej `databases` inställning kan vara konfigurerade endast under Skapa cache och endast med hjälp av PowerShell, CLI eller annan av hanteringsklienter. Ett exempel på hur du konfigurerar `databases` under Skapa cache med hjälp av PowerShell, se [ny AzureRmRedisCache](cache-howto-manage-redis-cache-powershell.md#databases).
> 
> 

<a name="maxclients"></a>
<sup>2</sup> `maxclients` är olika för varje Azure Redis-Cache prisnivån.

* Basic och Standard cacheminnen
  * C0 (250 MB) cache - too256 anslutningar
  * C1 (1 GB)-cache - in too1, 000 anslutningar
  * C2 (2,5 GB)-cache - in too2, 000 anslutningar
  * C3 (6 GB)-cache - in too5, 000 anslutningar
  * C4 (13 GB)-cache - in too10, 000 anslutningar
  * C5 (26 GB)-cache - in too15, 000 anslutningar
  * C6 (53 GB)-cache - in too20, 000 anslutningar
* Premium-cache
  * P1 (6 GB - 60 GB) - in too7, 500-anslutningar
  * P2 (13 GB - 130 GB) - upp too15 000 anslutningar
  * P3 (26 GB - 260 GB) - upp too30 000 anslutningar
  * P4 (53 GB - 530 GB) - upp too40 000 anslutningar

> [!NOTE]
> Med varje cachens storlek kan *upp till* ett visst antal anslutningar, varje anslutning tooRedis har associeras med den. Ett exempel på sådana kostnader är processor- och minnesanvändning på grund av TLS/SSL-kryptering. hello anslutningens maximala gränsen för en given cachestorlek förutsätter en lågt belastade cache. Om att läsa in från anslutningen kostnader *plus* belastningen från Klientåtgärder överskrider kapaciteten för hello system kan hello cache kan ha problem med kapacitet även om du inte har överskridit hello anslutningsgräns hello aktuella cachestorleken.
> 
> 



## <a name="redis-commands-not-supported-in-azure-redis-cache"></a>Redis-kommandon som inte stöds i Azure Redis-Cache
> [!IMPORTANT]
> Eftersom konfiguration och hantering av Azure Redis-Cache instanser hanteras av Microsoft, har hello följande kommandon inaktiverats. Om du försöker tooinvoke dem du får ett felmeddelande för`"(error) ERR unknown command"`.
> 
> * BGREWRITEAOF
> * BGSAVE
> * CONFIG
> * FELSÖKA
> * MIGRERA
> * SPARA
> * AVSTÄNGNING
> * SLAVEOF
> * KLUSTER - kluster skriva kommandon inaktiveras, men skrivskyddad klustret kommandon tillåts.
> 
> 

Mer information om Redis-kommandon finns [http://redis.io/commands](http://redis.io/commands).

## <a name="redis-console"></a>Redis-konsolen
Du kan på ett säkert sätt skicka kommandon tooyour Azure Redis-Cache-instanser som använder hello **Redis-konsolen**, som finns i hello Azure portal för alla nivåer i cacheminnet.

> [!IMPORTANT]
> - Hej Redis-konsolen fungerar inte med [VNET](cache-how-to-premium-vnet.md). När din cache är en del av ett VNET, endast klienter i hello VNET kan komma åt hello cache. Eftersom Redis-konsolen körs i din lokala webbläsare som är utanför hello VNET, kan den inte kan ansluta tooyour cache.
> - Inte alla Redis-kommandon stöds i Azure Redis-Cache. En lista över Redis-kommandon som är inaktiverat för Azure Redis-Cache, finns i föregående hello [Redis-kommandon som inte stöds i Azure Redis-Cache](#redis-commands-not-supported-in-azure-redis-cache) avsnitt. Mer information om Redis-kommandon finns [http://redis.io/commands](http://redis.io/commands).
> 
> 

tooaccess hello Redis-konsolen klickar du på **konsolen** från hello **Redis-Cache** bladet.

![Redis-konsolen](./media/cache-configure/redis-console-menu.png)

tooissue kommandon mot din cache-instans, bara typen hello önskade kommandot i hello-konsolen.

![Redis-konsolen](./media/cache-configure/redis-console.png)


### <a name="using-hello-redis-console-with-a-premium-clustered-cache"></a>Med hjälp av hello cache Redis-konsolen med en premium för

När cache med en premium hello Redis-konsolen för, kan du utfärda kommandon tooa enda Fragmentera hello-cache. tooissue ett kommando tooa specifika Fragmentera, först ansluta toohello önskade Fragmentera genom att klicka på hello Fragmentera väljare.

![Redis-konsolen](./media/cache-configure/redis-console-premium-cluster.png)

Om du försöker tooaccess en nyckel som lagras i en annan Fragmentera än hello anslutna Fragmentera får du ett fel meddelande liknande toohello efter meddelande.

```
shard1>get myKey
(error) MOVED 866 13.90.202.154:13000 (shard 0)
```

I föregående exempel hello Fragmentera 1 är hello valda Fragmentera, men `myKey` finns i Fragmentera 0, som anges av hello `(shard 0)` del av hello felmeddelande. I det här exemplet tooaccess `myKey`väljer Fragmentera 0 med hello Fragmentera Väljaren och problemet hello önskad kommando.


## <a name="move-your-cache-tooa-new-subscription"></a>Flytta den nya prenumerationen för cache-tooa
Du kan flytta den nya prenumerationen för cache-tooa genom att klicka på **flytta**.

![Flytta Redis-Cache](./media/cache-configure/redis-cache-move.png)

Information om hur du flyttar resurser från en resurs grupp tooanother och från en prenumeration tooanother finns [flytta resurser toonew resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md).

## <a name="next-steps"></a>Nästa steg
* Mer information om hur du arbetar med Redis-kommandon finns [hur kan jag köra Redis kommandon?](cache-faq.md#how-can-i-run-redis-commands)

