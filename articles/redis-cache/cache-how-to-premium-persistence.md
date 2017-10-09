---
title: "aaaHow tooconfigure datapersistence för Premium Azure Redis-Cache"
description: "Lär dig hur tooconfigure och hantera data beständiga din Premium-nivån Azure Redis-Cache-instanser"
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: b01cf279-60a0-4711-8c5f-af22d9540d38
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 08/24/2017
ms.author: sdanie
ms.openlocfilehash: 62feb6f5522e0270487f045eb303bf852434143d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-data-persistence-for-a-premium-azure-redis-cache"></a>Hur tooconfigure datapersistence för Premium Azure Redis-Cache
Azure Redis-Cache har olika cache-erbjudanden som ger flexibilitet i hello valet av cachestorlek och funktioner, inklusive funktioner för Premium-nivån, till exempel klustring, beständiga och stöd för virtuella nätverk. Den här artikeln beskriver hur tooconfigure beständiga i en premium Azure Redis-Cache instans.

Information om andra cache premium-funktioner finns [introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md).

## <a name="what-is-data-persistence"></a>Vad är beständiga data?
[Redis-persistence](https://redis.io/topics/persistence) kan du toopersist data som lagras i Redis. Du kan också ta ögonblicksbilder och säkerhetskopiera hello data som du kan läsa in vid maskinvarufel. Det här är en stor fördel över Basic eller standardnivån där alla hello data lagras i minnet och det kan finnas en potentiell dataförlust om ett fel uppstår där cachenoder är nere. 

Azure Redis-Cache erbjuder Redis-datapersistence med hjälp av följande modeller hello:

* **RDB beständiga** -när RDB (Redis-databas) beständiga är konfigurerad, Azure Redis-Cache kvarstår en ögonblicksbild av hello Redis-cache i en Redis binärformat toodisk baserat på en konfigurerbar säkerhetskopieringsfrekvens. Om ett allvarligt fel inträffar som inaktiverar såväl hello primär replik cache har hello cache byggts med hello senaste ögonblicksbild. Mer information om hello [fördelar](https://redis.io/topics/persistence#rdb-advantages) och [nackdelar](https://redis.io/topics/persistence#rdb-disadvantages) av RDB persistence.
* **AOF beständiga** -beständiga när AOF (Lägg till fil) har konfigurerats, Azure Redis-Cache sparar varje skrivning tooa åtgärdsloggen som har sparats minst en gång per sekund på ett Azure Storage-konto. Om ett allvarligt fel inträffar som inaktiverar såväl hello primär replik cache har hello cache byggts med hello lagras skrivåtgärder. Mer information om hello [fördelar](https://redis.io/topics/persistence#aof-advantages) och [nackdelar](https://redis.io/topics/persistence#aof-disadvantages) av AOF beständighet.

Beständiga konfigureras från hello **nytt Redis-Cache** bladet under Skapa cache och på hello **resurs menyn** befintliga premium cachelagrar.

[!INCLUDE [redis-cache-create](../../includes/redis-cache-premium-create.md)]

När en premium-prisnivån är markerad, klickar du på **Redis-persistence**.

![Redis-persistence][redis-cache-persistence]

hello steg i hello nästa avsnitt beskriver hur tooconfigure Redis-datapersistence på din nya premium-cachen. När Redis-persistence har konfigurerats klickar du på **skapa** toocreate din nya premium cachelagra med Redis-persistence.

## <a name="enable-redis-persistence"></a>Aktivera Redis-persistence

Redis-persistence har aktiverats på hello **Redis-datapersistence** bladet genom att välja antingen **RDB** eller **AOF** beständighet. För nya cacheminnen åt det här bladet under hello-cache skapas, enligt beskrivningen i föregående avsnitt i hello. Befintliga cacheminnen hello **Redis-datapersistence** bladet kan nås från hello **resurs menyn** för ditt cacheminne.

![Redis-inställningar][redis-cache-settings]


## <a name="configure-rdb-persistence"></a>Konfigurera RDB persistence

tooenable RDB beständiga klickar du på **RDB**. toodisable RDB beständiga på en tidigare aktiverade premium-cache, klickar du på **inaktiverade**.

![Redis-persistence RDB][redis-cache-rdb-persistence]

tooconfigure hello intervall för säkerhetskopiering, väljer en **säkerhetskopieringsfrekvens** hello nedrullningsbara listan. Alternativen är **15 minuter**, **30 minuter**, **60 minuter**, **6 timmar**, **12 timmar**, och **24 timmar**. Det här intervallet börjar räkna när hello tidigare säkerhetskopieringen har slutförts och när det ska gå en ny säkerhetskopia initieras.

Klicka på **Lagringskonto** tooselect hello storage-konto toouse och välj antingen hello **primärnyckel** eller **sekundärnyckeln** toouse från hello **lagring Nyckeln** listrutan. Du måste välja ett lagringskonto i hello samma region som hello cache, och en **Premiumlagring** konto rekommenderas eftersom premium-lagring har högre kapacitet. 

> [!IMPORTANT]
> Om hello lagringsnyckeln för ditt konto beständiga genereras, måste du konfigurera om hello önskade nyckeln från hello **Lagringsnyckel** listrutan.
> 
> 

Klicka på **OK** toosave hello beständiga konfigurationen.

hello nästa säkerhetskopiering (eller första säkerhetskopieringen för nya cacheminnen) initieras när långa hello säkerhetskopieringsfrekvens intervall.

## <a name="configure-aof-persistence"></a>Konfigurera AOF persistence

tooenable AOF beständiga klickar du på **AOF**. toodisable AOF beständiga på en tidigare aktiverade premium-cache, klickar du på **inaktiverade**.

![Redis-persistence AOF][redis-cache-aof-persistence]

tooconfigure AOF beständiga, ange en **första Lagringskonto**. Det här lagringskontot måste vara i hello samma region som hello cache, och en **Premium-lagring** konto rekommenderas eftersom premium-lagring har högre kapacitet. Du kan också konfigurera en ytterligare lagringskontonamnet **andra Lagringskonto**. Om ett andra storage-konto har konfigurerats, hello skrivningar toohello replik cache skrivs toothis andra storage-konto. För varje konfigurerad lagring-konto, väljer du antingen hello **primärnyckel** eller **sekundärnyckeln** toouse från hello **Lagringsnyckel** listrutan. 

> [!IMPORTANT]
> Om hello lagringsnyckeln för ditt konto beständiga genereras, måste du konfigurera om hello önskade nyckeln från hello **Lagringsnyckel** listrutan.
> 
> 

När AOF persistence har aktiverats kan du skriva operations toohello cache sparas toohello avses storage-konto (eller konton om du har konfigurerat ett andra storage-konto). I hello händelse av ett oåterkalleligt fel för som tar ned båda hello primära och repliken cache hello lagrade AOF loggen är toorebuild hello cache.

## <a name="persistence-faq"></a>Beständiga vanliga frågor och svar
hello innehåller följande lista svar toocommonly frågor och svar om Azure Redis-Cache-persistence.

* [Kan jag aktivera persistence på en tidigare skapad cache?](#can-i-enable-persistence-on-a-previously-created-cache)
* [Kan jag aktivera AOF och RDB beständiga på hello samtidigt?](#can-i-enable-aof-and-rdb-persistence-at-the-same-time)
* [Vilken beständiga modell ska jag välja?](#which-persistence-model-should-i-choose)
* [Vad händer om jag har skalats tooa annan storlek och en säkerhetskopia återställs som togs före hello skalning åtgärden?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)


### <a name="rdb-persistence"></a>RDB beständiga
* [Kan jag ändra hello RDB säkerhetskopieringsfrekvens när du har skapat hello cache?](#can-i-change-the-rdb-backup-frequency-after-i-create-the-cache)
* [Varför är det mer än 60 minuter mellan säkerhetskopieringar om jag har en RDB säkerhetskopieringsfrekvens 60 minuter?](#why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups)
* [Vad händer toohello gamla RDB säkerhetskopior när en ny säkerhetskopia görs?](#what-happens-to-the-old-rdb-backups-when-a-new-backup-is-made)

### <a name="aof-persistence"></a>AOF beständiga
* [När bör jag använda ett andra storage-konto?](#when-should-i-use-a-second-storage-account)
* [Stöder AOF beständiga påverkar hela svarstid, prestanda, och min-cache?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)
* [Hur tar jag bort hello andra storage-konto?](#how-can-i-remove-the-second-storage-account)
* [Vad är en omarbetning och hur påverkar min cache?](#what-is-a-rewrite-and-how-does-it-affect-my-cache)
* [Vad bör jag förvänta dig när skalning en cache med AOF aktiverad?](#what-should-i-expect-when-scaling-a-cache-with-aof-enabled)
* [Hur organiseras mina AOF data i lagring?](#how-is-my-aof-data-organized-in-storage)


### <a name="can-i-enable-persistence-on-a-previously-created-cache"></a>Kan jag aktivera persistence på en tidigare skapad cache?
Ja, Redis-persistence konfigureras skapa cache och på befintliga premium cacheminnen.

### <a name="can-i-enable-aof-and-rdb-persistence-at-hello-same-time"></a>Kan jag aktivera AOF och RDB beständiga på hello samtidigt?

Nej, du kan aktivera endast RDB eller AOF, men inte båda på hello samtidigt.

### <a name="which-persistence-model-should-i-choose"></a>Vilken beständiga modell ska jag välja?

AOF beständiga sparar varje skrivning tooa-loggen har vissa inverkan på dataflöde, jämfört med RDB beständiga som sparar säkerhetskopieringar utifrån hello konfigurerats säkerhetskopieringsintervallet, med minimal inverkan på prestanda. Välj AOF beständiga om din primära målet är toominimize dataförlust och du kan hantera en minskning i dataflöde för ditt cacheminne. Välj RDB beständiga om du vill att toomaintain optimal genomströmning på ditt cacheminne men ändå vill en mekanism för att återställa data.

* Mer information om hello [fördelar](https://redis.io/topics/persistence#rdb-advantages) och [nackdelar](https://redis.io/topics/persistence#rdb-disadvantages) av RDB persistence.
* Mer information om hello [fördelar](https://redis.io/topics/persistence#aof-advantages) och [nackdelar](https://redis.io/topics/persistence#aof-disadvantages) av AOF beständighet.

Läs mer på prestanda när du använder AOF beständiga [har AOF beständiga påverkar hela svarstid, prestanda, och min-cache?](#does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache)

### <a name="what-happens-if-i-have-scaled-tooa-different-size-and-a-backup-is-restored-that-was-made-before-hello-scaling-operation"></a>Vad händer om jag har skalats tooa annan storlek och en säkerhetskopia återställs som togs före hello skalning åtgärden?

För både RDB och AOF:

* Om du har skalats tooa större storlek påverkas inte.
* Om du har skalats tooa mindre storlek, och du har en anpassad [databaser](cache-configure.md#databases) inställningar som är större än hello [databaser gränsen](cache-configure.md#databases) för din nya storleken är inte återställa data i dessa databaser. Mer information finns i [är mina egna databaser anger berörda under skalning?](cache-how-to-scale.md#is-my-custom-databases-setting-affected-during-scaling)
* Om du har skalats tooa mindre storlek, och det finns inte tillräckligt med utrymme i hello mindre storlek toohold alla hello data från hello senaste säkerhetskopiering, nycklar kommer att avlägsnas under hello återställningsprocessen, vanligtvis med hello [allkeys lru](http://redis.io/topics/lru-cache) borttagning princip.

### <a name="can-i-change-hello-rdb-backup-frequency-after-i-create-hello-cache"></a>Kan jag ändra hello RDB säkerhetskopieringsfrekvens när du har skapat hello cache?
Ja, du kan ändra hello säkerhetskopieringsfrekvensen för RDB beständiga på hello **Redis-datapersistence** bladet. Instruktioner finns i [konfigurera Redis beständiga](#configure-redis-persistence).

### <a name="why-if-i-have-an-rdb-backup-frequency-of-60-minutes-there-is-more-than-60-minutes-between-backups"></a>Varför är det mer än 60 minuter mellan säkerhetskopieringar om jag har en RDB säkerhetskopieringsfrekvens 60 minuter?
hello RDB beständiga säkerhetskopieringsfrekvens intervall startar inte förrän hello tidigare säkerhetskopieringen har slutförts. Om hello frekvens för säkerhetskopiering är 60 minuter och det tar en säkerhetskopieringsprocessen 15 minuter toosuccessfully slutförts, startar inte hello nästa säkerhetskopiering tills 75 minuter efter hello starttid för hello tidigare säkerhetskopia.

### <a name="what-happens-toohello-old-rdb-backups-when-a-new-backup-is-made"></a>Vad händer toohello gamla RDB säkerhetskopior när en ny säkerhetskopia görs?
Alla RDB beständiga säkerhetskopior förutom hello senaste tas bort automatiskt. Borttagningen kan inte sker omedelbart men gamla säkerhetskopior bevaras inte under obestämd tid.


### <a name="when-should-i-use-a-second-storage-account"></a>När bör jag använda ett andra storage-konto?

Du bör använda ett andra storage-konto för AOF när du tror att du har högre än förväntade uppsättningen åtgärder på hello cache.  Konfigurera hello sekundära lagringskonto bidrar till ditt cacheminne inte nå lagringsgränser för bandbredd.

### <a name="does-aof-persistence-affect-throughout-latency-or-performance-of-my-cache"></a>Stöder AOF beständiga påverkar hela svarstid, prestanda, och min-cache?

AOF beständiga påverkar genomströmning av ungefär 15% – 20% när hello cache understiger största belastning (processor- och läsa in både under 90%). Det får inte vara problem med nätverkssvarstiden när hello cache är inom de angivna gränserna. Dock når hello cache dessa gränser snabbare med AOF aktiverad.

### <a name="how-can-i-remove-hello-second-storage-account"></a>Hur tar jag bort hello andra storage-konto?

Du kan ta bort hello AOF beständiga sekundära storage-konto genom att ange hello andra lagringskonto toobe hello samma som hello första lagringskontot. Instruktioner finns i [konfigurera AOF beständiga](#configure-aof-persistence).

### <a name="what-is-a-rewrite-and-how-does-it-affect-my-cache"></a>Vad är en omarbetning och hur påverkar min cache?

När hello AOF filen blir tillräckligt stor i kö på en omarbetning automatiskt på hello-cachen. hello omarbetning storlek hello AOF filen med hello minimal uppsättning åtgärder som krävs för toocreate hello aktuella datauppsättningen. Under omskrivningar, räknar tooreach prestandabegränsningarna snabbare särskilt vid hantering av stora datamängder. Omskrivningar ske mindre ofta som hello AOF filen blir större, men kan ta lång tid när det sker.

### <a name="what-should-i-expect-when-scaling-a-cache-with-aof-enabled"></a>Vad bör jag förvänta dig när skalning en cache med AOF aktiverad?

Om hello AOF filen när hello skalning är mycket stor förväntar du dig hello skala åtgärden tootake längre tid än förväntat eftersom den kommer att ladda filen hello efter skalning har slutförts.

Läs mer om att skala [vad händer om jag har skalats tooa annan storlek och en säkerhetskopia återställs som togs före hello skalning åtgärden?](#what-happens-if-i-have-scaled-to-a-different-size-and-a-backup-is-restored-that-was-made-before-the-scaling-operation)

### <a name="how-is-my-aof-data-organized-in-storage"></a>Hur organiseras mina AOF data i lagring?

Data som lagras i AOF filer är indelade i flera sidblobbar per nod tooincrease prestanda för att spara hello data toostorage. hello visar följande tabell hur många sidblobbar som ska användas för varje prisnivå:

| Premiumnivå | Blobar |
|--------------|-------|
| P1           | 4 per Fragmentera    |
| P2           | 8 per Fragmentera    |
| P3           | 16 per Fragmentera   |
| P4           | 20 per Fragmentera   |

När kluster är aktiverad, har varje Fragmentera i hello cache en egen uppsättning sidblobar, som anges i hello föregående tabell. Till exempel distribuerar en P2 cache med tre delar dess AOF fil över 24 sidblobbar (8 blobbar per Fragmentera med 3 shards).

Efter en omarbetning finns två uppsättningar av AOF filer i lagringen. Omskrivningar sker i bakgrunden hello och Lägg till toohello första uppsättning filer, medan uppsättning åtgärder som skickas toohello cache under hello omarbetning bifoga toohello andra uppsättningen. En säkerhetskopia lagras tillfälligt under omskrivningar om fel uppstår, men raderas omedelbart när en omarbetning har slutförts.


## <a name="next-steps"></a>Nästa steg
Lär dig hur toouse mer premium cachelagra funktioner.

* [Introduktion toohello Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md)

<!-- IMAGES -->

[redis-cache-premium-pricing-tier]: ./media/cache-how-to-premium-persistence/redis-cache-premium-pricing-tier.png

[redis-cache-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-persistence.png

[redis-cache-rdb-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-rdb-persistence.png

[redis-cache-aof-persistence]: ./media/cache-how-to-premium-persistence/redis-cache-aof-persistence.png

[redis-cache-settings]: ./media/cache-how-to-premium-persistence/redis-cache-settings.png
