---
title: "aaaHow tooconfigure Geo-replikering för Azure Redis-Cache | Microsoft Docs"
description: "Lär dig hur tooreplicate din Azure Redis-Cache instanser över geografiska regioner."
services: redis-cache
documentationcenter: 
author: steved0x
manager: douge
editor: 
ms.assetid: 375643dc-dbac-4bab-8004-d9ae9570440d
ms.service: cache
ms.workload: tbd
ms.tgt_pltfrm: cache-redis
ms.devlang: na
ms.topic: article
ms.date: 07/06/2017
ms.author: sdanie
ms.openlocfilehash: edcd6f202b51055d1a4e47ecaf11f9977d50aa81
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-geo-replication-for-azure-redis-cache"></a>Hur tooconfigure Geo-replikering för Azure Redis-Cache

GEO-replikering tillhandahåller en mekanism för att länka två instanser i Premium-nivån Azure Redis-Cache. En cache utses hello primära länkade cache och hello andra som sekundär länkade hello-cache. hello sekundär länkade cache skrivskyddas och data som skrivits toohello primära cache är replikeras toohello sekundära länkade cache. Den här funktionen kan vara används tooreplicate en cache i Azure-regioner. Den här artikeln innehåller en guide tooconfiguring Geo-replikering för din Premium-nivån Azure Redis-Cache-instanser.

## <a name="geo-replication-prerequisites"></a>Krav för GEO-replikering

tooconfigure Geo-replikering mellan två cacheminnen och hello följande krav måste uppfyllas:

- Båda cacheminnen måste vara [premiumnivån](cache-premium-tier-intro.md) cachelagrar.
- Båda cacheminnen måste vara i hello samma Azure-prenumeration.
- hello sekundär länkade cache måste vara antingen hello samma priser nivån eller en större prisnivå än hello primära länkade cache.
- Om hello primära länkade cache har aktiverad klustring, hello sekundär länkade cache måste ha aktiverad med hello klustring samma antal delar som hello primära länkade cache.
- Båda cache måste skapas och körs.
- Beständiga måste inte aktiveras på antingen cachen.
- GEO-replikering mellan Cacheminnena i hello stöds samma virtuella nätverk. GEO-replikering mellan Cacheminnena i olika Vnet stöds också, förutsatt att hello två Vnet har konfigurerats så att resurser i hello Vnet är kan tooreach varandra via TCP-anslutningar.

När Geo-replikering konfigureras hello följande begränsningar gäller tooyour länkade cache par:

- hello sekundära länkade cache är skrivskyddade. Du kan läsa från den, men du kan inte skriva alla data tooit. 
- Alla data som var i hello sekundära länkade cachen innan hello länken lades till tas bort. Om hello Geo-replikering tas därefter bort men replikeras hello data blir kvar hello sekundär länkade cache.
- Du kan inte påbörja en [skalning åtgärden](cache-how-to-scale.md) på antingen cachen eller [ändra hello antal shards](cache-how-to-premium-clustering.md) om hello cache har aktiverad klustring.
- Du kan inte aktivera persistence på antingen cachen.
- Du kan använda [exportera](cache-how-to-import-export-data.md#export) med antingen cache, men du kan bara [importera](cache-how-to-import-export-data.md#import) till hello primära länkade cache.
- Du kan inte ta bort länkade cache eller hello resursgruppen som innehåller dem, tills du tar bort hello Geo-replikering länk. Mer information finns i [varför hello misslyckas när jag försökte toodelete min länkade cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- Om hello två cacheminnen finns i olika regioner, gäller kostnader för nätverksegress toohello data replikeras mellan regioner toohello sekundära länkade cache. Mer information finns i [hur mycket finns det kosta tooreplicate Mina data i Azure-regioner?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- Det finns ingen automatisk redundans toohello sekundära länkade cache om hello primära cache (och dess replik) kraschar. I ordning toofailover klientprogram, behöver du toomanually ta bort hello Geo-replikering länk och punkt hello klienten program toohello cachelagra som tidigare var hello sekundär länkade cache. Mer information finns i [hur fungerar inte körs toohello sekundära länkade cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

## <a name="add-a-geo-replication-link"></a>Lägga till en länk för Geo-replikering

1. toolink två premium cachelagrar tillsammans för geo-replikering, klickar du på **Geo-replikering** hello resurs menyn hello-cache är avsedda som hello primära länkade cachelagra och klicka sedan på **Lägg till cache replikeringslänk**från hello **georeplikering** bladet.

    ![Lägga till en länk](./media/cache-how-to-geo-replication/cache-geo-location-menu.png)

2. Klicka på hello önskad Sekundär cache från hello hello namn **kompatibel cacheminnen** lista. Om din önskade cache inte visas i listan hello, kontrollera att hello [Geo-replikering krav](#geo-replication-prerequisites) för hello önskad Sekundär cache är uppfyllda. toofilter hello cacheminnen efter region, klickar du på hello önskad region i hello kartan toodisplay endast de cachelagrar i hello **kompatibel cacheminnen** lista.

    ![GEO-replikering kompatibel cacheminnen](./media/cache-how-to-geo-replication/cache-geo-location-select-link.png)
    
    Du kan också starta hello länkning process eller visa information om sekundära hello-cachen genom att använda hello snabbmenyn.

    ![Snabbmenyn för GEO-replikering](./media/cache-how-to-geo-replication/cache-geo-location-select-link-context-menu.png)

3. Klicka på **länk** toolink hello två cacheminnen tillsammans och börja hello replikeringen.

    ![Länken cachelagrar](./media/cache-how-to-geo-replication/cache-geo-location-confirm-link.png)

4. Du kan visa hello fortskrider hello replikeringen på hello **georeplikering** bladet.

    ![Länka status](./media/cache-how-to-geo-replication/cache-geo-location-linking.png)

    Du kan också visa hello länka status på hello **översikt** bladet för båda hello primära och sekundära cacheminnen.

    ![Cachestatus](./media/cache-how-to-geo-replication/cache-geo-location-link-status.png)

    När hello replikeringen är klar hello **länka status** ändras för**lyckades**.

    ![Cachestatus](./media/cache-how-to-geo-replication/cache-geo-location-link-successful.png)

    Hello primära länkade cache fortfarande är tillgänglig för användning under hello länka process, men hello sekundär länkade cache är inte tillgängligt förrän hello länka processen har slutförts.

## <a name="remove-a-geo-replication-link"></a>Ta bort en länk för Geo-replikering

1. tooremove hello länken mellan två cacheminnen och stoppa Geo-replikering, klickar du på **Avlänka cacheminnen** från hello **georeplikering** bladet.
    
    ![Avlänka cacheminnen](./media/cache-how-to-geo-replication/cache-geo-location-unlink.png)

    När hello bryta länkar är klart hello Sekundär cache är tillgänglig för både läsningar och skriver.

>[!NOTE]
>När hello Geo-replikering länken tas bort replikerade hello data från hello primära länkade cache blir kvar i hello Sekundär cache.
>
>

## <a name="geo-replication-faq"></a>Vanliga frågor om GEO-replikering

- [Kan jag använda Geo-replikering med en Standard- eller Basic-nivån cache?](#can-i-use-geo-replication-with-a-standard-or-basic-tier-cache)
- [Är Mina cacheminne tillgängliga för användning under hello länka eller bryta länkade processen?](#is-my-cache-available-for-use-during-the-linking-or-unlinking-process)
- [Kan jag koppla fler än två cacheminnen tillsammans?](#can-i-link-more-than-two-caches-together)
- [Kan jag koppla två cacheminnen från andra Azure-prenumerationer?](#can-i-link-two-caches-from-different-azure-subscriptions)
- [Kan jag koppla två cacheminnen med olika storlekar?](#can-i-link-two-caches-with-different-sizes)
- [Kan jag använda Geo-replikering med aktiverad klustring?](#can-i-use-geo-replication-with-clustering-enabled)
- [Kan jag använda Geo-replikering med min Cacheminnena i ett virtuellt nätverk?](#can-i-use-geo-replication-with-my-caches-in-a-vnet)
- [Kan jag använda PowerShell eller Azure CLI toomanage Geo-replikering?](#can-i-use-powershell-or-azure-cli-to-manage-geo-replication)
- [Hur mycket kostar det tooreplicate Mina data i Azure-regioner?](#how-much-does-it-cost-to-replicate-my-data-across-azure-regions)
- [Varför hello misslyckas när jag försökte toodelete min länkade cache?](#why-did-the-operation-fail-when-i-tried-to-delete-my-linked-cache)
- [Vilken region ska använda för min sekundära länkade cache?](#what-region-should-i-use-for-my-secondary-linked-cache)
- [Hur fungerar inte körs toohello sekundära länkade cache?](#how-does-failing-over-to-the-secondary-linked-cache-work)

### <a name="can-i-use-geo-replication-with-a-standard-or-basic-tier-cache"></a>Kan jag använda Geo-replikering med en Standard- eller Basic-nivån cache?

Nej, Geo-replikering är endast tillgängligt för Premium-nivån.

### <a name="is-my-cache-available-for-use-during-hello-linking-or-unlinking-process"></a>Är Mina cacheminne tillgängliga för användning under hello länka eller bryta länkade processen?

- När koppla ihop två cacheminnen för Geo-replikering, hello primära länkade cache fortfarande är tillgänglig för användning men hello sekundär länkade cache är inte tillgängligt förrän hello länka processen har slutförts.
- När du tar bort länken hello Geo-replikering mellan två cacheminnen är båda cacheminnen tillgängliga för användning.

### <a name="can-i-link-more-than-two-caches-together"></a>Kan jag koppla fler än två cacheminnen tillsammans?

Nej, när du använder Geo-replikering kan du bara länka två cacheminnen tillsammans.

### <a name="can-i-link-two-caches-from-different-azure-subscriptions"></a>Kan jag koppla två cacheminnen från andra Azure-prenumerationer?

Ingen, både cacheminnen måste vara i hello samma Azure-prenumeration.

### <a name="can-i-link-two-caches-with-different-sizes"></a>Kan jag koppla två cacheminnen med olika storlekar?

Ja, så länge hello sekundär länkade cache är större än hello primära länkade cache.

### <a name="can-i-use-geo-replication-with-clustering-enabled"></a>Kan jag använda Geo-replikering med aktiverad klustring?

Ja, så länge båda cacheminnen har hello samma antal delar.

### <a name="can-i-use-geo-replication-with-my-caches-in-a-vnet"></a>Kan jag använda Geo-replikering med min Cacheminnena i ett virtuellt nätverk?

Ja, Geo-replikering av Cacheminnena i Vnet stöds. 

- GEO-replikering mellan Cacheminnena i hello stöds samma virtuella nätverk.
- GEO-replikering mellan Cacheminnena i olika Vnet stöds också, förutsatt att hello två Vnet har konfigurerats så att resurser i hello Vnet är kan tooreach varandra via TCP-anslutningar.

### <a name="can-i-use-powershell-or-azure-cli-toomanage-geo-replication"></a>Kan jag använda PowerShell eller Azure CLI toomanage Geo-replikering?

Just nu kan endast hantera hello Geo-replikering med hjälp av Azure-portalen.

### <a name="how-much-does-it-cost-tooreplicate-my-data-across-azure-regions"></a>Hur mycket kostar det tooreplicate Mina data i Azure-regioner?

När du använder Geo-replikering är data från hello primära länkade cache replikerade toohello sekundära länkade cache. Om hello två länkade cacheminnen finns i hello samma Azure-regionen är gratis för hello dataöverföring. Om hello två länkad cacheminnen är i olika Azure-regioner är hello Geo-replikering data transfer kostnad hello bandbredd kostnaden för att replikera den data toohello andra Azure-region. Mer information finns i [bandbredd prisinformation](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="why-did-hello-operation-fail-when-i-tried-toodelete-my-linked-cache"></a>Varför hello misslyckas när jag försökte toodelete min länkade cache?

När två cacheminnen är länkade till varandra, kan du ta bort cache eller hello resursgruppen som innehåller dem förrän du tagit bort hello Geo-replikering länk. Om du försöker toodelete hello resursgruppen som innehåller en eller båda av hello länkade cacheminnen, hello andra resurser i resursgruppen hello tas bort, men hello resursgruppen förblir i hello `deleting` tillstånd och länkad Cacheminnena i hello resursgruppen. finns kvar i hello `running` tillstånd. toocomplete hello borttagning av hello resursgrupp och hello länkade cacheminnen inom den, ta bort hello Geo-replikering länk som beskrivs i [ta bort en länk för Geo-replikering](#remove-a-geo-replication-link).

### <a name="what-region-should-i-use-for-my-secondary-linked-cache"></a>Vilken region ska använda för min sekundära länkade cache?

I allmänhet det rekommenderas för din cache tooexist i hello samma Azure-region som hello-program som har åtkomst till den. Om programmet har en primär och en återställningsplats region, ska din primära och sekundära cacheminnen finnas i samma regioner. Mer information om parad regioner finns [metodtips – parad Azure-regioner](../best-practices-availability-paired-regions.md).

### <a name="how-does-failing-over-toohello-secondary-linked-cache-work"></a>Hur fungerar inte körs toohello sekundära länkade cache?

I hello första versionen av Geo-replikering stöder Azure Redis-Cache inte automatisk redundans över Azure-regioner. GEO-replikering används framför allt för en katastrofåterställning. I en distater återställningsscenario ska kunder få upp hello hel programstack i en säkerhetskopiering region i ett samordnat sätt i stället för att låta enskilda programkomponenter bestämma när tooswitch tootheir säkerhetskopieringar på egen hand. Detta är särskilt relevanta tooRedis. En av hello viktiga fördelar med Redis är att det är en mycket låg latens store. Om Redis används av ett program inte över tooa olika Azure-region men hello beräkning nivå inte, hello läggs avrunda resa tid skulle ha en märkbar effekt på prestanda. Därför vill vi tooavoid Redis misslyckas över automatiskt på grund av problem med tootransient tillgänglighet.

För närvarande tooinitiate hello redundans måste tooremove hello Geo-replikering länken i hello Azure-portalen och ändra sedan hello anslutning slutpunkt i hello Redis-klient från hello primära länkade cache toohello (tidigare länkad) Sekundär cache. När två cacheminnen är koppla bort, hello hello replik blir en vanlig skrivskyddad cache igen och accepterar begäranden direkt från Redis-klienter.


## <a name="next-steps"></a>Nästa steg

Mer information om hello [Azure Redis Cache Premium-nivån](cache-premium-tier-intro.md).

