---
title: aaaMonitor enheten StorSimple 8000-serien | Microsoft Docs
description: "Beskriver hur toouse hello StorSimple Enhetshanteraren tjänsten toomonitor användning, i/o-prestanda och för kapacitetsanvändning."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2017
ms.author: alkohli
ms.openlocfilehash: 092dab8dd301c50fc12316b4031a8d1b34fab876
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomonitor-your-storsimple-device"></a>Använd hello StorSimple Enhetshanteraren service toomonitor din StorSimple-enhet
## <a name="overview"></a>Översikt
Du kan använda hello StorSimple Enhetshanteraren service toomonitor specifika enheter i din StorSimple-lösning. Du kan skapa anpassade diagram baserat på i/o-prestanda, kapacitetsutnyttjande, dataflödet i nätverket och prestandamått för enheten och fästa dessa toohello instrumentpanelen. Mer information finns för[anpassa instrumentpanelen portal](/articles/azure-portal/azure-portal-dashboards.md).

tooview hello övervakningsinformation för en specifik enhet i hello Azure-portalen, Välj hello StorSimple enheten Manager-tjänsten. Hello listan över enheter och välj din enhet och gå sedan för**övervakaren**. Du kan sedan se hello **kapacitet**, **användning**, och **prestanda** diagram för hello valda enheten.

## <a name="capacity"></a>Kapacitet
**Kapacitet** spårar hello allokerade utrymmet och hello kvar på hello enhet. hello återstående kapacitet visas som lokalt Fäst eller nivåer.

hello etablerad och återstående kapacitet är ytterligare fördelade på nivåindelade och lokalt fästa volymer. För varje volym hello etablerad kapacitet och hello återstående kapacitet på hello enhet visas.

![I/o-kapacitet](./media/storsimple-8000-monitor-device/device-capacity.png)



## <a name="usage"></a>Användning
**Användning** spårar mått relaterade toohello mängden lagringsutrymme för data som används av hello volymer, volymbehållare eller enhet. Du kan skapa rapporter utifrån hello kapacitetsutnyttjande din primära lagring, molnlagringen eller enhetens utrymme. Kapacitetsutnyttjande kan mätas på en viss volym, en specifik volymbehållare eller alla volymbehållare.
Som standard rapporteras hello användning för de senaste 24 timmarna. Du kan redigera hello diagram toochange hello varaktighet över vilka hello rapporteras användning genom att välja från:
* Senaste 24 timmarna
* De senaste 7 dagarna
* De senaste 30 dagarna
* De senaste 90 dagarna
* Senaste året


hello primära molnet och lokal lagring används kan beskrivas på följande sätt:

### <a name="primary-storage-usage"></a>Lagringskvoten på primär
Följande diagram visar hello mängden data som skrivs tooStorSimple volymer innan hello data är deduplicerad och komprimeras. Du kan visa hello primära lagringsutrymme som används av alla volymer i en volymbehållare eller för en enskild volym. hello primära lagringsutrymme som används är ytterligare fördelade på primära nivåindelade lagringsutrymme som används och primära lokalt Fäst lagring används.

hello visar följande diagram hello primära lagringsutrymme som används för en StorSimple-enheten innan och efter att en ögonblicksbild i molnet skapades. Eftersom det är bara volymdata bör en ögonblicksbild i molnet inte ändra hello primära lagringsplatsen. Som du ser visar hello diagrammet någon skillnad i hello primära nivåindelade eller lokalt Fäst lagringsutrymme som används på grund av en ögonblicksbild i molnet. hello ögonblicksbild i molnet startade ungefär 11:50 pm på enheten.

![Primär kapacitetsutnyttjande efter ögonblicksbild i molnet](./media/storsimple-8000-monitor-device/device-primary-storage-after-cloudsnapshot.png)

Om du nu köra i/o på hello värddatorn ansluten tooyour StorSimple-enhet, ska du se en ökning i primära skiktad lagring eller primär lokalt Fäst lagringsutrymme som används beroende på vilka volymer (nivåer eller lokalt Fäst) skriver du hello data till. Här följer hello primära lagringsplatsen användning diagram för en StorSimple-enhet. På den här enheten skriver hello StorSimple värden igång betjänar på cirka 2:30 pm på en nivåindelad volym på hello enhet. Du kan se hello belastning i motsvarande toohello-i/o körs på värd hello hello skriva byte/s.

![Prestanda när den körs på i/o nivåindelade volymer](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

Om du tittar på hello primära nivåindelade lagringsutrymme som används, som har gått in medan hello primära lokalt Fäst användning förblir oförändrad som det finns delgetts inga skrivningar toohello lokalt fästa volymer hello enhet.

![Primär kapacitetsutnyttjande när i/o körs på nivåindelade volymer](./media/storsimple-8000-monitor-device/device-primary-storage-io-from-initiator.png)

Om du kör uppdatering 3 eller högre, du kan dela upp hello primära lagringskapacitetsutnyttjandet av en enskild volym, alla volymer, alla nivåindelade volymer och alla lokalt fästa volymer som visas nedan. Bryta ned av alla lokalt fästa volymer kan du tooquickly fastställa hur mycket av hello lokala nivån används.

![Primär kapacitetsutnyttjande för alla nivåindelade volymer](./media/storsimple-8000-monitor-device/monitor-usage3.png)

![Primär kapacitetsutnyttjande för alla lokalt fästa volymer](./media/storsimple-8000-monitor-device/monitor-usage4.png)

Du kan ytterligare klickar du på alla hello volymer i hello lista och se hello motsvarande användning.

![Primär kapacitetsutnyttjande för alla lokalt fästa volymer](./media/storsimple-8000-monitor-device/device-primary-storage-usage-by-volume.png)

### <a name="cloud-storage-usage"></a>Lagringskvoten på molnet
Följande diagram visar hello mängden molnlagring som används. Dessa data är deduplicerad och komprimeras. Den här mängden innehåller molnögonblicksbilder som kan innehålla data som inte visas i alla primära volymer och sparas i äldre eller obligatoriskt kvarhållning syfte. Du kan jämföra hello primära och molnet lagring förbrukning siffrorna tooget en uppfattning om hello data minskning Betygsätt även om hello-numret är inte exakt.

hello följande diagram visar hello molnet lagringsanvändningen av StorSimple-enhet när en ögonblicksbild i molnet togs.

* hello ögonblicksbild i molnet som startades vid ungefär 11:50:00 på enheten och du kan se som innan hello ögonblicksbild i molnet, det fanns inga molnlagring som används. 
* En gång hello ögonblicksbild i molnet slutförts, hello molnet lagringsanvändningen tog upp 0,89 GB. 
* När hello ögonblicksbild i molnet pågick finns även en motsvarande belastning i hello IO från enheten toocloud.

    ![Molnet lagringsanvändningen innan ögonblicksbild i molnet](./media/storsimple-8000-monitor-device/device-cloud-storage-before-cloudsnapshot.png)

    ![Lagring av molnanvändning efter ögonblicksbild i molnet](./media/storsimple-8000-monitor-device/device-cloud-storage-after-cloudsnapshot.png)

    ![IO från enheten toocloud under en ögonblicksbild i molnet](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)


### <a name="local-storage-usage"></a>Användning av lokal lagring
Följande diagram visar hello total användning för hello-enhet, vilket överstiger lagringskvoten på primära eftersom den innehåller linjär hello SSD-nivån. Det här skiktet innehåller en datamängd som även finns på hello enhetens andra nivåer. hello kapacitet i linjär hello SSD-nivån kopplas så att när nya data kommer in, hello gamla data har flyttats toohello hårddisknivå (då den är deduplicerad och komprimerade) och därefter toohello moln.

Över tiden nivåer primära lagring som används och lokal lagring som används mest sannolikt ökar tillsammans tills hello data börjar toobe toohello moln. Vid den punkten hello lokal lagring används börjar förmodligen tooplateau, men ökar hello primära lagringsutrymme som används när mer data skrivs.

hello visar följande diagram hello primära lagringsutrymme som används för en StorSimple-enhet när en ögonblicksbild i molnet togs. hello ögonblicksbild i molnet som startade kl 11:50 och hello lokal lagring igång minskar vid den tiden. hello lokal lagring används minskade från 1.445 GB too1.09 GB. Detta anger att troligen hello okomprimerad data i hello linjär SSD-nivån har dedupliceras, komprimeras och flyttas till hello hårddisknivå. Observera att om hello enheten har redan en stor mängd data både hello SSD och HDD-nivåer, du inte kan se denna minskning. I det här exemplet har hello enhet en liten mängd data.

![Lokal lagring användning efter ögonblicksbild i molnet](./media/storsimple-8000-monitor-device/device-local-storage-after-cloudsnapshot.png)

## <a name="performance"></a>Prestanda
**Prestanda** spårar mått relaterade toohello antalet Läs- och skrivåtgärder mellan antingen hello iSCSI-initieraren gränssnitt på hello värdservern och hello enhet eller hello enheten och hello molnet. Prestanda kan beräknas för en viss volym, en specifik volymbehållare eller alla volymbehållare. Prestanda innehåller CPU-användning och nätverksgenomflöde för hello olika nätverksgränssnitt på enheten.

### <a name="io-performance-for-initiator-toodevice"></a>I/o-prestanda för initieraren toodevice
hello diagrammet nedan visar hello-i/o för hello initieraren tooyour enhetens alla hello volymer för en enhet för produktion. hello mått ritas är läsa och skriva byte per sekund. Du kan också skapa diagram över Läs-, Skriv- och utestående I/O eller läsa och skriva svarstider.

![I/o-prestanda för initieraren toodevice](./media/storsimple-8000-monitor-device/device-io-from-initiator.png)

### <a name="io-performance-for-device-toocloud"></a>I/o-prestanda för enheten toocloud
För hello samma enhet hello i/o-åtgärder ritas för hello data från hello enhet toohello moln för alla hello volymbehållare. På den här enheten hello data är bara i hello linjär nivå och inget har hamnat toohello moln. Det finns ingen läs-och skrivåtgärder som från enhet toohello moln. Hello toppar i hello diagrammet är därför vid ett intervall på 5 minuter som motsvarar toohello frekvens som kontrolleras pulsslag hello mellan hello enheten och hello-tjänsten.

För hello samma enhet, en moln-ögonblicksbild skapades för volymdata Starta kl 11:50. Detta resulterade i data som flödar från hello enhet toohello moln. Skrivning har sett toohello moln i varaktigheten. hello-i/o-diagrammet visar en topp hello skriva byte/s motsvarande toohello tidpunkt när hello ögonblicksbilden togs.

![IO från enheten toocloud under en ögonblicksbild i molnet](./media/storsimple-8000-monitor-device/device-io-to-cloud-during-cloudsnapshot.png)

### <a name="network-throughput-for-device-network-interfaces"></a>Dataflödet i nätverket för enheten nätverksgränssnitt
**Dataflödet i nätverket** spårar mått relaterade toohello mängden data som överförs från hello iSCSI-initieraren nätverksgränssnitt på hello värdservern och hello enheten och mellan hello enheten och hello moln. Du kan övervaka det här måttet för varje nätverksgränssnitt för hello iSCSI på enheten.

Hej följande diagram visar hello dataflödet i nätverket för hello Data 0, 1 1 GbE nätverket på din enhet som har båda moln-aktiverat (standard) och iSCSI-aktiverade. På den här enheten på 14 juni cirka 21: 00 var data nivåer i hello-moln (utan ögonblicksbilder som utförts på moln som tid punkter tootiering är hello mekanism toomove hello data i hello moln) som resulterade i i/o hanteras toohello moln. Hello nätverket genomströmning diagrammet är en motsvarande belastning för hello samtidigt och de flesta av hello nätverkstrafiken är utgående toohello moln.

![Dataflödet i nätverket för Data 0](./media/storsimple-8000-monitor-device/device-network-throughput-data0.png)

Om vi tittar på hello Data 1 gränssnittet genomströmning diagram, en annan 1 GbE nätverksgränssnittet som var bara iSCSI-aktiverade och sedan uppstod när praktiskt taget nätverkstrafiken varaktigheten.

![Dataflödet i nätverket för Data 1](./media/storsimple-8000-monitor-device/device-network-throughput-data1.png)


## <a name="cpu-utilization-for-device"></a>CPU-belastningen för enhet
**Processoranvändningen** spårar mått relaterade toohello CPU används på enheten. hello följande diagram visar hello CPU-användning statistik för en enhet i produktion.

![CPU-belastningen för enhet](./media/storsimple-8000-monitor-device/device-cpu-utilization.png)



## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hello StorSimple Enhetshanteraren service enheten instrumentpanel](storsimple-device-dashboard.md).
* Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

