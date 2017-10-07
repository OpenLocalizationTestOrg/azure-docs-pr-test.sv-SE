---
title: "aaaStorSimple Snapshot Manager säkerhetskopieringsjobb | Microsoft Docs"
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen tooview och hantera schemalagda pågående och slutförda säkerhetskopieringsjobb."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: bf4dcff6-c819-4766-b9d9-9922831cb200
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 3dba0a2aa527d17d67130f537bcdce5722b05a76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-tooview-and-manage-backup-jobs"></a>Använda StorSimple Snapshot Manager tooview och hantera säkerhetskopieringsjobb

## <a name="overview"></a>Översikt
Hej **jobb** nod i hello **omfång** visar hello **schemalagda**, **senaste 24 timmarna**, och **kör**säkerhetskopiera uppgifter som du har initierat interaktivt eller av en konfigurerade principen. 

Den här självstudiekursen beskrivs hur du kan använda hello **jobb** nod toodisplay information om schemalagda senaste och pågående säkerhetskopieringsjobb. (hello lista över jobb och motsvarande information visas i hello **resultat** fönstret.) Du kan dessutom högerklickar du på ett listade jobb och se en snabbmeny som visar tillgängliga åtgärder.

## <a name="view-scheduled-jobs"></a>Visa schemalagda jobb
Använd följande procedur tooview schemalagda säkerhetskopieringsjobb hello.

#### <a name="tooview-scheduled-jobs"></a>tooview schemalagda jobb
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager. 
2. I hello **omfång** rutan Expandera hello **jobb** och klicka **schemalagda**. hello visas följande information i hello **resultat** fönstret:
   
   * **Namnet** – hello namn på hello schemalagda ögonblicksbild
   * **Kör nästa gång** – hello datum och tid för hello nästa schemalagda ögonblicksbild
   * **Senaste körning** – hello datum och tid för hello senaste schemalagda ögonblicksbild
     
     > [!NOTE]
     > En enda ögonblicksbilder hello **nästa** och **senaste kör** blir hello samma.
     
     ![Schemalagda säkerhetskopieringsjobb](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_scheduled.png) 
3. tooperform ytterligare åtgärder på ett specifikt jobb, högerklicka på hello Jobbnamnet i hello **resultat** fönstret och välj bland hello menyalternativen.

## <a name="view-recent-jobs"></a>Visa senaste jobb
Använd hello följa proceduren tooview säkerhetskopiering och återställning jobb som har slutförts i hello senaste 24 timmarna.

#### <a name="tooview-recent-jobs"></a>tooview senaste jobb
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **jobb** och klicka **senaste 24 timmarna**. Hej **resultat** visar säkerhetskopieringsjobb för hello senaste 24 timmarna (tooa högst 64 jobb). hello visas följande information i hello **resultat** rutan, beroende på hello **visa** alternativ som du anger:
   
   * **Namnet** – hello namn på hello schemalagda ögonblicksbild.
   * **Starta** – hello datum och tid då hello ögonblicksbild påbörjades.
   * **Stoppats** – hello datum och tid när hello ögonblicksbild slutförts eller avslutats.
   * **Förfluten tid** – hello tidslängd mellan hello **igång** och **stoppad** gånger.
   * **Status för** – hello tillståndet för hello nyligen slutföra jobbet. **Lyckade** anger hello säkerhetskopian skapades. **Det gick inte** anger hello jobbet inte kördes.
   * **Information** – hello hello felet.
   * **Byte bearbetas (MB)** – hello mängd data från hello volym grupp som har bearbetats (i MB). 
     
     ![Jobb som körts i hello senaste 24 timmarna](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_Last_24_hours.png) 
3. tooperform ytterligare åtgärder på ett specifikt jobb, högerklicka på hello Jobbnamnet i hello **resultat** fönstret och välj bland hello menyalternativen.
   
    ![Ta bort ett jobb](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Delete_backup.png)

## <a name="view-currently-running-jobs"></a>Visa pågående jobb
Använd hello följa proceduren tooview jobb som körs för närvarande.

#### <a name="tooview-currently-running-jobs"></a>tooview jobb som körs för närvarande
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **jobb** och klicka **kör**. Beroende på hello **visa** alternativ som du anger hello följande information visas i hello **resultat** fönstret:
   
   * **Namnet** – hello namn på hello schemalagda ögonblicksbild.
   * **Starta** – hello datum och tid då hello ögonblicksbild påbörjades.
   * **Kontrollpunkt** – hello aktuell åtgärd av hello säkerhetskopiering.
   * **Status för** – hello procent färdigt.
   * **Förfluten tid** – hello mängden tid som har gått sedan hello säkerhetskopiering påbörjades. 
   * **Genomsnittlig genomströmning (MB)** – förhållandet mellan totalt antal bearbetade data toothat total tid för bearbetning av (MB) byte.
   * **Byte bearbetas (MB)** – Totalt antal byte av data som bearbetas (i MB).
   * **Byte som skrivits (MB)** – Totalt antal byte skrevs (i MB). Den inkluderar hello data samt hello metadata och därför är vanligtvis större än hello byte bearbetas.
     
     ![Jobb som körs](./media/storsimple-snapshot-manager-manage-backup-jobs/HCS_SSM_Jobs_running.png)
3. tooperform ytterligare åtgärder på ett specifikt jobb, högerklicka på hello Jobbnamnet i hello **resultat** fönstret och välj bland hello menyalternativen.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).
* Lär dig hur för[använder StorSimple Snapshot Manager toomanage hello säkerhetskopieringskatalogen](storsimple-snapshot-manager-manage-backup-catalog.md).

