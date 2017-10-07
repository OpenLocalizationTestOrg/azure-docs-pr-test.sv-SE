---
title: aaaMonitor din StorSimple-enhet | Microsoft Docs
description: "Beskriver hur toouse hello StorSimple Manager-tjänsten toomonitor i/o-prestanda, kapacitetsutnyttjande, dataflödet i nätverket och Enhetsprestanda."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: bd4f7704-4f6f-47d0-927a-b1c91eabc453
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/16/2016
ms.author: alkohli
ms.openlocfilehash: c1f614a7f52728650bfadb3335435b8b5a17e6ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomonitor-your-storsimple-device"></a>Använd hello StorSimple Manager-tjänsten toomonitor din StorSimple-enhet
## <a name="overview"></a>Översikt
Du kan använda hello StorSimple Manager-tjänsten toomonitor specifika enheter i din StorSimple-lösning. Du kan skapa anpassade diagram baserat på i/o-prestanda, kapacitetsutnyttjande, dataflödet i nätverket och prestandamått för enheten. 

tooview hello övervakningsinformation för en specifik enhet i hello klassiska Azure-portalen väljer hello StorSimple Manager-tjänsten. Klicka på hello **övervakaren** och välj sedan från hello lista över enheter. Hej **övervakaren** innehåller hello följande information.

## <a name="io-performance"></a>I/o-prestanda
**I/o-prestanda** spårar mått relaterade toohello antalet Läs- och skrivåtgärder mellan antingen hello iSCSI-initieraren gränssnitt på hello värdservern och hello enhet eller hello enheten och hello molnet. Prestanda kan beräknas för en viss volym, en specifik volymbehållare eller alla volymbehållare.

hello diagrammet nedan visar hello-i/o för hello initieraren tooyour enhetens alla hello volymer för en enhet för produktion. hello mått ritas läses och skriva byte per sekund, läsa och skriva i/o-åtgärder per sekund, och läsa och skriva svarstider.

![I/o-prestanda från initieraren toodevice](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_InitiatorTODevice_For_AllVolumesM.png)

För hello samma enhet hello i/o-åtgärder ritas för hello data från hello enhet toohello moln för alla hello volymbehållare. På den här enheten hello data är bara i hello linjär nivå och inget har hamnat toohello moln. Det finns ingen läs-och skrivåtgärder som från enhet toohello moln. Hello toppar i hello diagrammet är därför vid ett intervall på 5 minuter som motsvarar toohello frekvens som kontrolleras pulsslag hello mellan hello enheten och hello-tjänsten. 

![I/o-prestanda från enheten toocloud](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainersM.png)

För hello samma enhet, en moln-ögonblicksbild skapades för volymdata som börjar på 2:00 pm. Detta resulterade i data som flödar från hello enhet toohello moln. Läsning / skrivning har sett toohello moln i varaktigheten. hello IO diagrammet visar en topp i hello olika mått motsvarande toohello tid när hello ögonblicksbilden togs. 

![I/o-prestanda för enheten toocloud efter ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_IO_Performance_For_DeviceTOCloud_For_AllVolumeContainers2M.png)

## <a name="capacity-utilization"></a>Kapacitetsutnyttjande
**Kapacitetsutnyttjande** spårar mått relaterade toohello mängden lagringsutrymme för data som används av hello volymer, volymbehållare eller enhet. Du kan skapa rapporter utifrån hello kapacitetsutnyttjande din primära lagring, molnlagringen eller enhetens utrymme. Kapacitetsutnyttjande kan mätas på en viss volym, en specifik volymbehållare eller alla volymbehållare.

hello primära molnet och enheten lagringskapacitet kan beskrivas på följande sätt:

### <a name="primary-storage-capacity-utilization"></a>Primär lagringskapacitetsutnyttjandet
Följande diagram visar hello mängden data som skrivs tooStorSimple volymer innan hello data är deduplicerad och komprimeras. Du kan visa hello primära lagringsanvändningen av alla volymer eller för en enskild volym.

När du visar hello primära lagringsplatsen volym kapacitet användning diagram för alla volymer jämfört med varje hello enskilda volymerna och summan av hello primära data i båda dessa fall kan finnas det ett matchningsfel mellan hello två tal. hello totala primära data på alla volymer kan inte lägga till upp toohello totalsumman av hello primära data hello enskilda volymer. Orsaken kan vara tooone av hello följande:

* **Ögonblicksbild av data som ingår för alla volymer**: det här beteendet visas bara om du använder version tidigare än uppdatering 3. hello primära data som visas för alla hello volymer är hello summan av hello primära data för varje volym och hello ögonblicksbilddata. hello primära data som visas för en viss volym motsvarar tooonly hello mängden data som allokerats på hello volymen (och innehåller inte hello motsvarande ögonblicksbild volymdata).
  
    Detta kan också förklaras av hello följande formel:
  
    *Primära data (alla volymer) = summan av (primära data (volym i) + storleken på data från ögonblicksbilder (volume i))*
  
    *Om det primära data (volym i) = storlek för primära data allokerade toovolume jag*
  
    Om hello-ögonblicksbilder tas bort via hello-tjänsten, görs hello borttagning asynkront i hello bakgrund. Det kan ta lite tid innan hello volym data storlek toobe uppdateras följande hello ögonblicksbild tas bort. 
  
    Om du kör uppdatering 3 eller senare, ingår inte hello ögonblicksbilddata i hello volymdata. Och hello primära användningen beräknas enligt följande:
  
    * Primära data (alla volymer) = summan av (primära data (volym i)
  
    *Om det primära data (volym i) = storlek för primära data allokerade toovolume jag*
* **Volymerna som ingår i alla volymer med övervakning inaktiveras**: Om du har volymer på enheten som övervakningen inaktiverades hello övervakningsdata för volymerna enskilda blir inte tillgängliga i hello diagram. Hello-data för alla volymer i hello diagrammet innehåller emellertid hello volymer där övervakning har inaktiverats. 
* **Ta bort volymer med levande tillhörande säkerhetskopior inkluderade för alla volymer**: om volymer som innehåller ögonblicksbilddata för tas bort men hello associerade ögonblicksbilder fortfarande finns och sedan kan du se ett matchningsfel.
* **Ta bort volymerna för alla volymer**: I vissa fall gamla volymer finns även om dessa har tagits bort. hello effekten av borttagning visas inte och hello enheten kan visa lägre tillgänglig kapacitet. Du måste toocontact Microsoft-supporten tooremove volymerna.

hello visar följande diagram hello primära lagringskapacitetsutnyttjandet av StorSimple-enhet innan och efter att en ögonblicksbild i molnet skapades. Eftersom det är bara volymdata bör en ögonblicksbild i molnet inte ändra hello primära lagringsplatsen. Som du ser visar hello diagrammet någon skillnad i hello primära kapacitetsutnyttjande på grund av en ögonblicksbild i molnet. hello ögonblicksbild i molnet startade cirka 2:00 pm på enheten.

![Primär kapacitetsutnyttjande innan ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes2M.png)

![Primär kapacitetsutnyttjande efter ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_PrimaryCapacityUtil_For_AllVolumes1M.png)

Om du kör uppdatering 2 eller högre, du kan dela upp hello primära lagringskapacitetsutnyttjandet av en enskild volym, alla volymer, alla nivåindelade volymer och alla lokalt fästa volymer som visas nedan. Bryta ned av alla lokalt fästa volymer kan du tooquickly fastställa hur mycket av hello lokala nivån används.

![Primär kapacitetsutnyttjande för alla lokalt fästa volymer](./media/storsimple-monitor-device/localvolumes.png)

### <a name="cloud-storage-capacity-utilization"></a>Lagringskapacitet för molnet
Följande diagram visar hello mängden molnlagring som används. Dessa data är deduplicerad och komprimeras. Den här mängden innehåller molnögonblicksbilder som kan innehålla data som inte visas i alla primära volymer och sparas i äldre eller obligatoriskt kvarhållning syfte. Du kan jämföra hello primära och molnet lagring förbrukning siffrorna tooget en uppfattning om hello data minskning Betygsätt även om hello-numret är inte exakt. hello visar följande diagram hello molnet lagringskapacitetsutnyttjandet av StorSimple-enhet innan och efter att en ögonblicksbild i molnet skapades. hello ögonblicksbild i molnet startades cirka 2:00 pm på enheten och du kan se hello molnet kapacitetsutnyttjande tog när hello samma tid som ökar från 5.73 MB too4.04 GB.

![Molnet kapacitetsutnyttjande innan ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers2M.png)

![Kapacitet för molnanvändning efter ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_CloudCapacityUtil_For_AllVolumeContainers1M.png)

### <a name="device-storage-capacity-utilization"></a>Lagringskapacitet för enhet
Följande diagram visar hello total användning för hello enheten, vilket kommer att mer än primära lagringsanvändningen eftersom den innehåller linjär hello SSD-nivån. Det här skiktet innehåller en datamängd som även finns på hello enhetens andra nivåer. hello kapacitet i linjär hello SSD-nivån kopplas så att när nya data kommer in, hello gamla data har flyttats toohello hårddisknivå (då den är deduplicerad och komprimerade) och därefter toohello moln.

Över tiden, primära kapacitetsutnyttjande och enheten kapacitetsutnyttjande kommer troligen att öka tillsammans tills hello data börjar toobe nivåer toohello moln. Vid den punkten hello enheten kapacitetsutnyttjande börjar förmodligen tooplateau men hello primära kapacitetsutnyttjande ökar eftersom mer detaljerade data lagras.

hello visar följande diagram hello primära lagringskapacitetsutnyttjandet av StorSimple-enhet innan och efter att en ögonblicksbild i molnet skapades. hello ögonblicksbild i molnet som startade kl. 2:00 pm och hello enheten kapacitetsutnyttjande igång minskar vid den tiden. hello enheten lagringskapacitetsutnyttjandet minskade från 11.58 GB too7.48 GB. Detta anger att troligen hello okomprimerad data i hello linjär SSD-nivån har dedupliceras, komprimeras och flyttas till hello hårddisknivå. Observera att om hello enheten har redan en stor mängd data både hello SSD och HDD-nivåer, du inte kan se denna minskning. I det här exemplet har hello enhet en liten mängd data.

![Enheten kapacitetsutnyttjande innan ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil2M.png)

![Enheten kapacitetsutnyttjande efter ögonblicksbild i molnet](./media/storsimple-monitor-device/StorSimple_DeviceCapacityUtil1M.png)

## <a name="network-throughput"></a>Dataflödet i nätverket
**Dataflödet i nätverket** spårar mått relaterade toohello mängden data som överförs från hello iSCSI-initieraren nätverksgränssnitt på hello värdservern och hello enheten och mellan hello enheten och hello moln. Du kan övervaka det här måttet för varje nätverksgränssnitt för hello iSCSI på enheten.

hello följande diagram visar hello dataflödet i nätverket för hello Data 0 och 4 för Data, både 1 GbE-nätverkskort på enheten. I den här instansen aktiverades Data 0 cloud-medan Data 4 har iSCSI-aktiverade. Du kan se både hello inkommande och utgående trafik för din StorSimple-enhet hello. hello platt linje i diagrammet för hello från 15:24:00 på grund av toohello fakta vi samla in data endast var femte minut och bör ignoreras. 

![Dataflödet i nätverket för Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data0M.png)

![Dataflödet i nätverket för Data4](./media/storsimple-monitor-device/StorSimple_NetworkThroughput_Data4M.png)

## <a name="device-performance"></a>Enhetsprestanda
**Enhetsprestanda** spårar mått relaterade toohello prestandan för din enhet. hello följande diagram visar hello CPU-användning statistik för en enhet i produktion.

![CPU-belastningen för enhet](./media/storsimple-monitor-device/StorSimple_DeviceMonitor_DevicePerformance1M.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hello StorSimple Manager-tjänsten enhet instrumentpanel](storsimple-device-dashboard.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

