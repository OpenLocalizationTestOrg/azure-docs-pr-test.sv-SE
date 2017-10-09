---
title: "Azure Redis Cache Premium-nivån för aaaIntroduction toohello | Microsoft Docs"
description: "Lär dig hur toocreate och hantera Redis-Persistence Redis-kluster och virtuella nätverk stöd för din Premium-nivån Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 30f46f9f-e6ec-4c38-a8cc-f9d4444856e5
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: sdanie
ms.openlocfilehash: 5b58a03647fbf1198509ac6f1acd04f1b682ad95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toohello-azure-redis-cache-premium-tier"></a>Introduktion toohello Azure Redis Cache Premium-nivån
Azure Redis-Cache är en distribuerad, hanterade cache som hjälper dig att skapa mycket skalbart och dynamisk program genom att snabbt komma åt tooyour data. 

hello nya Premium-nivån är en klar nivå Enterprise, som innehåller alla hello Standard skikt funktioner och fler, till exempel bättre prestanda, större arbetsbelastningar, katastrofåterställning, importera och exportera och förbättrad säkerhet. Fortsätt läsa toolearn mer om hello ytterligare funktioner för hello premiumnivån cache.

## <a name="better-performance-compared-toostandard-or-basic-tier"></a>Bättre prestanda jämfört med tooStandard eller grundläggande nivån
**Bättre prestanda över Standard eller grundläggande nivån.** Cacheminnena i hello Premium-nivån har distribuerats på maskinvara, vilket har snabbare processorer och ger bättre prestanda jämfört med toohello Basic eller Standard-nivån. Premium-nivån cacheminnen har högre genomflöde och lägre latens. 

**Dataflöde för hello samma storlek Cache är högre upp i Premium som jämfört med tooStandard nivå.** Till exempel hello genomströmning 53 GB P4 (Premium) cache är 250K begäranden per sekund som jämfört med too150K för C6 (Standard).

Mer information om storlek, genomflöde och bandbredd med premium cacheminnen finns [Azure Redis-Cache vanliga frågor och svar](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## <a name="redis-data-persistence"></a>Redis-datapersistence
Hej premiumnivån tillåter toopersist hello cachelagrade data i Azure Storage-konto. Alla hello-data lagras i en grundläggande/Standard cache endast i minnet. Om underliggande infrastruktur kan problem det vara potentiell dataförlust. Vi rekommenderar att funktionen hello Redis data beständiga i hello Premium-nivån tooincrease återhämtning mot dataförlust. Azure Redis-Cache erbjuder RDB och AOF (kommer snart) alternativ i [Redis-persistence](http://redis.io/topics/persistence). 

Anvisningar om hur du konfigurerar persistence finns [hur tooconfigure persistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md).

## <a name="redis-cluster"></a>Redis-kluster
Om du vill toocreate cacheminnen större än 53 GB eller vill tooshard data över flera Redis-noder, kan du använda Redis-kluster som är tillgängliga i hello Premium-nivån. Varje nod består av ett par för cache av primära/replik som hanteras av Azure för hög tillgänglighet. 

**Redis-kluster ger dig maximal skala och genomflöde.** Genomströmning ökar linjärt när du ökar hello antalet delar (noder) i hello kluster. T.ex. Om du skapar ett kluster P4 av 10 shards kommer hello tillgängliga dataflöde är 250K * 10 = 2,5 miljoner förfrågningar per sekund. Se hello [Azure Redis-Cache vanliga frågor och svar](cache-faq.md#what-redis-cache-offering-and-size-should-i-use) för mer information om storlek, genomflöde och bandbredd med premium cacheminnen.

tooget igång med klustring, se [hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md).

## <a name="enhanced-security-and-isolation"></a>Förbättrad säkerhet och isolering
Cacheminnet som skapats i hello Basic eller Standard-nivån är tillgängliga på hello offentliga internet. Komma åt toohello Cache är begränsad baserat på hello snabbtangent. Se till att endast klienter i en angiven nätverket kan komma åt hello Cache med hello Premium-nivån kan du ytterligare. Du kan distribuera Redis-Cache i en [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/). Du kan använda alla hello funktioner för virtuella nätverk som undernät, principer för åtkomstkontroll och andra funktioner toofurther begränsa åtkomst tooRedis.

Mer information finns i [hur tooconfigure virtuella nätverket har stöd för Premium Azure Redis-Cache](cache-how-to-premium-vnet.md).

## <a name="importexport"></a>Import/Export
Import/Export är en Azure Redis-Cache data management-åtgärd som gör att du tooimport data till Azure Redis-Cache eller exportera data från Azure Redis-Cache genom att importera och exportera en ögonblicksbild för Redis-Cache databasen (RDB) från en premium cache tooa sidblob i en Azure Storage-konto. Detta aktiverar du toomigrate mellan olika instanser av Azure Redis-Cache eller fylla hello cachen med data innan de används.

Importera kan vara används toobring Redis kompatibel RDB filer från en Redis-server som kör i molnet eller miljö, inklusive Redis som körs på Linux, Windows eller valfri provider som molntjänster, till exempel Amazon Web Services och andra. Importera data är ett enkelt sätt toocreate en cache med data i förväg. Under importen hello Azure Redis-Cache läser in hello RDB filer från Azure storage i minnet och infogar hello nycklar i hello cache.

Exportera kan tooexport hello data som lagras i Azure Redis-Cache tooRedis kompatibel RDB fil(er). Du kan använda den här funktionen toomove data från en Azure Redis-Cache-instansen tooanother eller tooanother Redis-servern. Under exportprocessen hello skapas en temporär fil på hello VM att värdar hello Azure Redis-Cache server-instansen och hello fil överförda toohello avses storage-konto. När hello exporten har slutförts med antingen en status för lyckad eller misslyckad tas hello tillfälliga filen bort.

Mer information finns i [hur tooimport data till och exportera data från Azure Redis-Cache](cache-how-to-import-export-data.md).

## <a name="reboot"></a>Starta om
hello premium-nivån kan du tooreboot en eller flera noder i din cache på begäran. Detta gör att du tootest ditt program för återhämtning i hello händelse av fel. Du kan starta om hello följande noder.

* Huvudnod i ditt cacheminne
* Underordnade noder i ditt cacheminne
* Huvud- och underordnade noder i ditt cacheminne
* När du använder en premium-cache med kluster måste du starta om hello master, slavserver eller båda noderna för enskilda delar i hello cache

Mer information finns i [omstart](cache-administration.md#reboot) och [omstart vanliga frågor och svar](cache-administration.md#reboot-faq).

>[!NOTE]
>Starta om funktionen har nu aktiverats för alla nivåer för Azure Redis-Cache.
>
>

## <a name="schedule-updates"></a>Schemauppdateringar
hello schemalagda uppdateringar kan du toodesignate en underhållsperiod för ditt cacheminne. När hello underhållsperiod har angetts görs alla uppdateringar för Redis-servern under den här perioden. toodesignate en underhållsperiod, Välj önskad hello dagar och ange hello fönstret starttid för underhåll för varje dag. Observera att hello underhållsfönstertiden i UTC. 

Mer information finns i [schemalägga uppdateringar](cache-administration.md#schedule-updates) och [Schemalägg uppdateringar vanliga frågor och svar](cache-administration.md#schedule-updates-faq).

> [!NOTE]
> Endast Redis-server uppdateringar görs under hello schemalagda underhållsperiod. Hej underhållsfönstret gäller inte tooAzure uppdateringar eller uppdaterar toohello VM-operativsystemet.
> 
> 

## <a name="geo-replication"></a>Geo-replikering

**GEO-replikering** tillhandahåller en mekanism för att länka två instanser i Premium-nivån Azure Redis-Cache. En cache utses hello primära länkade cache och hello andra som sekundär länkade hello-cache. hello sekundär länkade cache skrivskyddas och data som skrivits toohello primära cache är replikeras toohello sekundära länkade cache. Den här funktionen kan vara används tooreplicate en cache i Azure-regioner.

Mer information finns i [hur tooconfigure Geo-replikering för Azure Redis-Cache](cache-how-to-geo-replication.md).


## <a name="tooscale-toohello-premium-tier"></a>tooscale toohello premium-nivån
tooscale toohello premium-nivån, bara välja något av hello premiumnivå i hello **ändra prisnivån** bladet. Du kan även skala din cache toohello premium-nivån med PowerShell och CLI. Stegvisa instruktioner finns [hur tooScale Azure Redis-Cache](cache-how-to-scale.md) och [hur tooautomate skalning åtgärden](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## <a name="next-steps"></a>Nästa steg
Skapa ett cacheminne och utforska hello nya premium-nivån funktioner.

* [Hur tooconfigure persistence för Premium Azure Redis-Cache](cache-how-to-premium-persistence.md)
* [Hur tooconfigure virtuella nätverket har stöd för Premium Azure Redis-Cache](cache-how-to-premium-vnet.md)
* [Hur tooconfigure klustring för Premium Azure Redis-Cache](cache-how-to-premium-clustering.md)
* [Hur tooimport data till och exportera data från Azure Redis-Cache](cache-how-to-import-export-data.md)
* [Hur tooadminister Azure Redis-Cache](cache-administration.md)

