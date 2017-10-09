---
title: aaaView och hantera virtuella StorSimple-matris jobb | Microsoft Docs
description: "Beskriver hello StorSimple Enhetshanteraren jobb sida och hur toouse den tootrack senaste och aktuella jobb för hello virtuella StorSimple-matris."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 31879821-b599-4609-a7f4-d4b0f658a933
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 11/11/2016
ms.author: alkohli
ms.openlocfilehash: cf3f3e7bcdfff0ff2328b7354db2482286800e93
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-tooview-jobs-for-hello-storsimple-virtual-array"></a>Använda hello StorSimple Enhetshanteraren service tooview jobb för hello virtuella StorSimple-matris
## <a name="overview"></a>Översikt
Hej **jobb** bladet innehåller en enda central portal för att visa och hantera jobb som har startats på virtuella lagringsmatriser som är anslutna tooyour StorSimple enheten Manager-tjänsten. Du kan visa körs slutförda och misslyckade jobb för flera virtuella enheter. Resultat visas i tabellformat.

![Jobb-bladet](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)

Du kan snabbt hitta hello jobb som du är intresserad av genom att filtrera på fält som:

* **Tidsintervallet** – jobb kan filtreras baserat på hello datum och ett tidsintervall.
* **Enheter** – jobb startas på en specifik enhet ansluten tooyour tjänst. hello filtrerade jobben visas sedan som en tabell baserad på hello följande attribut:
  
  * **Namnet** – hello Jobbnamnet kan vara **alla**, **säkerhetskopiering**, **klona**, **växla över**, **hämta uppdateringar** , eller **installera uppdateringar**.
  * **Status för** – jobb kan vara **alla**, **pågår**, **lyckades**, eller **misslyckades**, eller **avbruten**.
  * **Entiteten** – hello jobb kan vara kopplad till en volym, resurs eller enhet.
  * **Enheten** – hello namn på hello enhet på vilken hello-jobbet startades.
  * **Startats på** – hello tid då hello-jobbet startades.
  * **Varaktighet** – hello varaktighet för i vilken hello jobbet kördes.
* **Status för** – du kan söka efter alla, körs slutförda och misslyckade jobb.
* **Jobbtyp** – hello jobbtypen kan all, säkerhetskopiering, återställning, redundans, ladda ned uppdateringar, eller installera uppdateringar.

hello lista över jobb uppdateras var 30: e sekund.

## <a name="view-job-details"></a>Visa jobbinformation
Utföra hello följande steg tooview hello information för alla jobb.

#### <a name="tooview-job-details"></a>tooview jobbinformation
1. På hello **jobb** bladet, visa hello jobb som du är intresserad av genom att köra en fråga med lämpliga filter. Du kan söka efter slutförda eller inte körs jobb.
2. Välj ett jobb hello tabular listan över jobb.
   
    ![Jobb-bladet](./media/storsimple-virtual-array-manage-jobs/ova-jobs-blade.png)
3. Hello längst hello-sidan, klickar du på **information**.
4. I hello **information** dialogrutan kan du visa status, detaljer och statistik. hello följande bild visar ett exempel på hello **säkerhetskopiering jobbinformation** dialogrutan.
   
    ![Jobbinformation](./media/storsimple-virtual-array-manage-jobs/ova-jobs-details.png)

#### <a name="job-failures-when-hello-virtual-machine-is-paused-in-hello-hypervisor"></a>Jobbfel när hello virtuella datorn är pausad i hello hypervisor-program
När ett jobb är i statusen för din virtuella StorSimple-matris och hello enhet (virtuell dator etablerats i hypervisor-program) har pausats av fler än 15 minuter, hello jobb misslyckas. Detta på grund av tooyour virtuella StorSimple-matris tid inte är synkroniserad med hello Microsoft Azure. 

Du ser hello följande fel: ”din enhet tid är inte synkroniserat med Microsoft Azure hello med mer än 15 minuter. Se till att hello hypervisor och hello enheten gånger synkroniseras med en NTP-servern. Kontrollera att det inte finns några anslutningsproblem. tootroubleshoot anslutningsproblem kör diagnostiktest från hello lokala webbgränssnittet för din virtuella enhet ”.

Dessa fel gäller toobackup, återställning, uppdatering och växling vid fel jobb. Om den virtuella datorn har etablerats i Hyper-V, synkroniserar hello datorn slutligen tid med din hypervisor. När det händer kan du starta om jobbet.

## <a name="next-steps"></a>Nästa steg
[Lär dig hur hello toouse lokala web UI tooadminister din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

