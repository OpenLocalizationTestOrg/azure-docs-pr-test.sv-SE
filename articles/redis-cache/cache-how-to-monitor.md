---
title: aaaHow toomonitor Azure Redis-Cache | Microsoft Docs
description: "Lär dig hur toomonitor hello hälsotillstånd och prestanda Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 7e70b153-9c87-4290-85af-2228f31df118
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: sdanie
ms.openlocfilehash: c474d485dfcbb109d5bb634a980f6db080598e13
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-azure-redis-cache"></a>Hur toomonitor Azure Redis-Cache
Azure Redis-Cache använder [Azure-Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/) tooprovide flera alternativ för övervakning av cacheinstanser. Du kan visa mått, Fäst metrics diagram toohello startsidan, anpassa hello datum och tid intervallet för övervakning av diagram, lägga till och ta bort mått från hello diagram och Ställ in aviseringar när vissa villkor är uppfyllda. Dessa verktyg aktiverar du toomonitor hello hälsotillståndet för dina Azure Redis-Cache-instanser och hjälper dig att hantera dina cachelagring program.

Mätvärden för Azure Redis-Cache instanser samlas in med hjälp av hello Redis [INFO](http://redis.io/commands/info) kommandot ungefär två gånger per minut och automatiskt lagras i 30 dagar (se [exportera cache mått](#export-cache-metrics) tooconfigure en olika bevarandeprincip) så att de kan visas i hello mått diagram och utvärdera Varningsregler. Mer information om hello olika INFO värdena som används för varje cache-mått finns [tillgängliga mått och rapportering intervall](#available-metrics-and-reporting-intervals).

<a name="view-cache-metrics"></a>

tooview cache mått [Bläddra](cache-configure.md#configure-redis-cache-settings) tooyour cache-instans i hello [Azure-portalen](https://portal.azure.com).  Azure Redis-Cache innehåller vissa inbyggda diagram om hello **översikt** bladet och hello **Redis mått** bladet. Varje diagram kan anpassas genom att lägga till eller ta bort mått och ändra hello reporting intervall.

![Redis-mått](./media/cache-how-to-monitor/redis-cache-redis-metrics-blade.png)

## <a name="view-pre-configured-metrics-charts"></a>Visa förkonfigurerade mått diagram

Hej **översikt** bladet har hello följande förkonfigurerade övervakning diagram.

* [Övervakning av diagram](#monitoring-charts)
* [Diagram för användning](#usage-charts)

### <a name="monitoring-charts"></a>Övervakning av diagram
Hej **övervakning** avsnitt i hello **översikt** bladet har **träffar och missar**, **hämtar och anger**, **anslutningar**, och **totala kommandon** diagram.

![Övervakning av diagram](./media/cache-how-to-monitor/redis-cache-monitoring-part.png)

### <a name="usage-charts"></a>Diagram för användning
Hej **användning** avsnitt i hello **översikt** bladet har **Redis-serverbelastning**, **minnesanvändning**, **nätverksbandbredd** , och **CPU-användning** diagram och visar även hello **prisnivå** för hello cache-instans.

![Diagram för användning](./media/cache-how-to-monitor/redis-cache-usage-part.png)

Hej **prisnivå** visar hello cache priser nivå och kan användas för[skala](cache-how-to-scale.md) hello cache tooa olika prisnivån.

## <a name="view-metrics-with-azure-monitor"></a>Visa mått med Azure Övervakare
tooview Redis mått och skapa anpassade diagram med hjälp av Azure-Monitor klickar du på **mått** från hello **resurs menyn**, och anpassa ditt diagram med hjälp av hello önskad mätvärden, rapportering intervall, diagram, och Mer.

![Redis-mått](./media/cache-how-to-monitor/redis-cache-monitor.png)

Mer information om hur du arbetar med hjälp av Azure-Monitor finns [översikt över mått i Microsoft Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md).

<a name="how-to-view-metrics-and-customize-chart"></a>
<a name="enable-cache-diagnostics"></a>
## <a name="export-cache-metrics"></a>Exportera cache-mått
Som standard cache i Azure-Monitor är [lagras i 30 dagar](../monitoring-and-diagnostics/monitoring-overview-azure-monitor.md#store-and-archive) och sedan tas bort. toopersist din cache-mätvärden för längre än 30 dagar kan du [ange ett lagringskonto](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) och ange en **bevarande (dagar)** princip för cache-mått. 

tooconfigure ett lagringskonto för cache-mått:

1. Klicka på **diagnostik** från hello **resurs menyn** i hello **Redis-Cache** bladet.
2. Klicka på **på**.
3. Kontrollera **Arkivera tooa lagringskonto**.
4. Välj hello storage-konto i vilka toostore hello cache mått.
5. Kontrollera hello **1 minut** kryssrutan och ange en **bevarande (dagar)** princip. Om du inte vill tooapply alla bevarandeprincip och behålla data alltid ange **bevarande (dagar)** för**0**.
6. Klicka på **Spara**.

![Redis-diagnostik](./media/cache-how-to-monitor/redis-cache-diagnostics.png)

>[!NOTE]
>I tillägg tooarchiving din mått toostorage cache kan du också [strömma dem tooan Event hub eller skicka dem tooLog Analytics](../monitoring-and-diagnostics/monitoring-overview-metrics.md#export-metrics).
>
>

tooaccess din statistik och, du kan visa dem i hello Azure-portalen enligt beskrivningen i den här artikeln och du kan också komma åt dem med hjälp av hello [Azure övervakaren mått REST API](../monitoring-and-diagnostics/monitoring-overview-metrics.md#access-metrics-via-the-rest-api).

> [!NOTE]
> Om du ändrar lagringskonton hello data på hello som tidigare har konfigurerat lagringskonto fortfarande är tillgänglig för nedladdning, men visas inte i hello Azure-portalen.  
> 
> 

## <a name="available-metrics-and-reporting-intervals"></a>Tillgängliga mått och rapportering intervall
Cache-mått rapporteras med flera reporting intervall, inklusive **efter timme**, **idag**, **föregående vecka**, och **anpassad**. Hej **mått** bladet för varje mått diagram visar hello genomsnittlig, lägsta och högsta värden för varje mått i hello diagram och vissa mått Visa totalt för hello reporting intervall. 

Varje mått innehåller två versioner. En mätvärdet mäter prestanda för hello hela cachen och cacheminnen som använder [clustering](cache-how-to-premium-clustering.md), andra versionen av hello mått som innehåller `(Shard 0-9)` hello namn åtgärder prestanda för en enskild Fragmentera i cache. Till exempel om en cache har 4 shards `Cache Hits` hello totala mängden träffar för hela hello-cache och `Cache Hits (Shard 3)` är bara hello träffar för att fragmentera hello-cache.

> [!NOTE]
> Du kan även när hello cache är inaktiv utan anslutna aktiv klient-program, för att se vissa Cacheaktivitet, till exempel anslutna klienter, minnesanvändning och åtgärder som utförs. Den här aktiviteten är normalt under hello driften av en Azure Redis-Cache-instans.
> 
> 

| Mått | Beskrivning |
| --- | --- |
| Träffar i cache |hello antal lyckade sökningar under hello angivna reporting intervallet som nyckel. Det här mappar för`keyspace_hits` från hello Redis [INFO](http://redis.io/commands/info) kommando. |
| Cachemissar |hello antal misslyckade viktiga sökningar under hello angivna reporting intervallet. Det här mappar för`keyspace_misses` från hello Redis information på. Cachemissar innebär inte nödvändigtvis att det finns ett problem med hello-cachen. Till exempel när du använder hello cache-reservera programming mönster, ett program att leta i hello cache för ett objekt. Om hello objektet inte är det (cache-miss), hämtas från databasen hello hello objektet och till toohello cache för nästa gång. Cachemissar är normalt att hello cache-reservera programming mönster. Om hello antal cachemissar är högre än väntat, granska hello programlogik som fyller och läser från hello cache. Om objekt är att avlägsnas från hello cache på grund av toomemory tryck sedan det kan finnas vissa cachemissar, men en bättre mått toomonitor för minnesbelastning skulle vara `Used Memory` eller `Evicted Keys`. |
| Anslutna klienter |hello antal klientcachen anslutningar toohello under hello angivna reporting intervallet. Det här mappar för`connected_clients` från hello Redis information på. En gång hello [anslutningsgränsen](cache-configure.md#default-redis-server-configuration) uppnås efterföljande anslutningsförsök misslyckas toohello cache. Observera att även om det finns inga aktiva klientprogram, det kan fortfarande finnas några instanser av anslutna klienter på grund av toointernal processer och anslutningar. |
| Avlägsnade nycklar |hello antal objekt som avlägsnas från cache hello under hello visst reporting intervall på grund av toohello `maxmemory` gränsen. Det här mappar för`evicted_keys` från hello Redis information på. |
| Utgångna nycklar |hello antal objekt som har upphört att gälla från hello-cache under hello angivna reporting intervallet. Det här värdet mappar för`expired_keys` från hello Redis information på. |
| Totalt antal nycklar  | hello maximalt antal nycklar i hello-cache under hello tidigare reporting tidsperiod. Det här mappar för`keyspace` från hello Redis information på. På grund av tooa begränsning av hello underliggande mått system för med aktiverad klustring, returnerar Totalt antal nycklar hello maximalt antal nycklar hello Fragmentera som hade hello maximalt antal nycklar under hello reporting intervall.  |
| Hämtar |hello antal get-åtgärder från hello-cache under hello angivna reporting intervallet. Det här värdet är hello summan av hello följande värden från hello Redis INFO alla kommandot: `cmdstat_get`, `cmdstat_hget`, `cmdstat_hgetall`, `cmdstat_hmget`, `cmdstat_mget`, `cmdstat_getbit`, och `cmdstat_getrange`, och motsvarande toohello summan av cacheträffar och missar under hello reporting intervall. |
| Redis-serverbelastning |hello procentandelen cykler vilka hello Redis-servern är upptagen och inte väntar på inaktiv för meddelanden. Om räknaren når 100 hello Redis-servern har uppnått ett tak för prestanda och hello CPU kan inte bearbeta innebär att fungera någon snabbare. Om du ser hög Redis-serverbelastning ser du tidsgränsfel hello-klienten. I det här fallet bör du skala upp eller partitionering dina data till flera cacheminnen. |
| Anger |hello antalet set operations toohello cache under hello angetts reporting intervall. Det här värdet är hello summan av hello följande värden från hello Redis INFO alla kommandot: `cmdstat_set`, `cmdstat_hset`, `cmdstat_hmset`, `cmdstat_hsetnx`, `cmdstat_lset`, `cmdstat_mset`, `cmdstat_msetnx`, `cmdstat_setbit`, `cmdstat_setex`, `cmdstat_setrange` , och `cmdstat_setnx`. |
| Totalt antal åtgärder |hello Totalt antal kommandon som bearbetas av hello cacheserver under hello angetts reporting intervall. Det här värdet mappar för`total_commands_processed` från hello Redis information på. Observera att när Azure Redis-Cache används enbart för pub/sub det några mått för `Cache Hits`, `Cache Misses`, `Gets`, eller `Sets`, men det finns `Total Operations` mått som visar hello cache-användning för pub/sub-åtgärder. |
| Använt minne |hello mängden cacheminne som används för nyckel/värde-par i hello-cachen i MB under hello angetts reporting intervall. Det här värdet mappar för`used_memory` från hello Redis information på. Detta inkluderar inte metadata eller fragmentering. |
| Använt minne RSS |hello mängden cacheminne som används i MB under hello angivna reporting intervallet, inklusive fragmentering och metadata. Det här värdet mappar för`used_memory_rss` från hello Redis information på. |
| Processor |hello CPU-användning av hello Azure Redis-Cache-server i procent under hello angetts reporting intervall. Det här värdet mappar toohello operativsystemet `\Processor(_Total)\% Processor Time` prestandaräknare. |
| Läs i cache |hello mängd data läses från hello cacheminnet i megabyte per sekund (MB/s) under hello angetts reporting intervall. Det här värdet är härledd från hello nätverkskort som stöder hello virtuell dator som är värd för hello cache och är inte Redis specifika. **Det här värdet motsvarar toohello nätverksbandbredd som används av det här cacheminnet. Om du vill tooset in aviseringar för server side nätverket bandbreddsgränser sedan skapa den med den här `Cache Read` räknaren. Se [tabellen](cache-faq.md#cache-performance) för hello observerats bandbreddsgränser för olika cache priser nivåer och storlekar.** |
| Cache-skrivåtgärder |Hej mängden data som skrivs toohello cache i megabyte per sekund (MB/s) under hello angivna reporting intervallet. Det här värdet är härledd från hello nätverkskort som stöder hello virtuell dator som är värd för hello cache och är inte Redis specifika. Det här värdet motsvarar toohello bandbredd på data som skickas toohello cache hello-klient. |

<a name="operations-and-alerts"></a>
## <a name="alerts"></a>Aviseringar
Du kan konfigurera tooreceive varningar baserat på mått och aktivitet. Azure övervakaren kan en avisering toodo hello efter när den utlöser tooconfigure:

* Skicka ett e-postmeddelande
* anropa en webhook
* Anropa en Azure Logikapp

tooconfigure Varningsregler för ditt cacheminne, klickar du på **Varna regler** från hello **resurs menyn**.

![Övervakning](./media/cache-how-to-monitor/redis-cache-monitoring.png)

Mer information om hur du konfigurerar och använder aviseringar finns [översikt över aviseringar](../monitoring-and-diagnostics/insights-alerts-portal.md).

## <a name="activity-logs"></a>Aktivitetsloggar
Aktivitetsloggar ger kunskaper om hello-åtgärder som utfördes på Azure Redis-Cache-instanser. Det som tidigare kallades ”granskningsloggar” eller ”arbetsloggarna”. Använda aktivitetsloggar kan du bestämma hello ”vad, som, och när” för alla skrivåtgärder (PUT, POST, ta bort) vidtas på Azure Redis-Cache-instanser. 

> [!NOTE]
> Aktivitetsloggar omfattar inte läsåtgärder (GET).
>
>

tooview aktivitetsloggar för ditt cacheminne, klickar du på **aktivitetsloggar** från hello **resurs menyn**.

Läs mer om aktivitetsloggar [översikt över hello Azure-aktivitetsloggen](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).











