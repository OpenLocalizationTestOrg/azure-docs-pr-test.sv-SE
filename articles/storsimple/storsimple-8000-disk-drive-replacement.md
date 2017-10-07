---
title: "aaaReplace en enhet på en enhet för StorSimple 8000-serien | Microsoft Docs"
description: "Förklarar hur tooreplace en disk enhet på en primär StorSimple-enhet eller en EBOD bilaga."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 073/2017
ms.author: alkohli
ms.openlocfilehash: 09c1bf5e97adf54a609a868e921351bc63e00a83
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-a-disk-drive-on-your-storsimple-8000-series-device"></a>Ersätta en diskenhet på enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur du kan ta bort och ersätter en felaktig eller misslyckade hårddisk på en Microsoft Azure StorSimple-enhet. tooreplace en enhet, måste du:

* Koppla ur hello antitamper Lås
* Ta bort hello diskenhet
* Installera hello ersättning diskenhet

> [!IMPORTANT]
> Innan du tar bort och ersätter en diskenhet granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).
 

## <a name="disengage-hello-antitamper-lock"></a>Koppla ur hello antitamper Lås
Här beskrivs hur hello antitamper lås på din StorSimple-enhet kan vara aktiverad eller inaktiverad när du ersätter hello diskenheter. Hej antitamper Lås monteras i hello enhet operatör hanterar och de kan nås via en liten öppning under hello spärren hello referensen. Enheter levereras med hello Lås set toohello låst position.

#### <a name="toounlock-hello-antitamper-lock"></a>toounlock hello antitamper Lås
1. Infoga noggrant hello lock-tangenten (en ”tamperproof” T10 för som tillhandahålls av Microsoft) till hello öppning i hello referensen och till en socket. 
   
   Hello Röd indikator är synlig i hello öppning om hello antitamper lock är aktiverat.
  
    ![Låst enhet](./media/storsimple-disk-drive-replacement/IC741056.png)
   
    **Bild 1** anti manipulationsindikerande Lås används
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Öppning av indikator |
   | 2 |Antitamper Lås |
2. Rotera hello nyckel i en moturs riktning tills hello red indikatorn inte visas i hello öppning ovan hello nyckel.
3. Ta bort hello nyckel.
   
    ![Olåsta diskenhet](./media/storsimple-disk-drive-replacement/IC741057.png)
   
    **Bild 2** olåst diskenhet
4. hello diskenhet kan nu tas bort.

Hello åtgärderna i omvänd tooengage hello Lås.

## <a name="remove-hello-disk-drive"></a>Ta bort hello diskenhet
Din StorSimple-enhet stöder en RAID 10-liknande blanksteg lagringskonfiguration. Detta innebär att den fungerar normalt med en skadad disk SSD-enhet (SSD) eller hårddisk enhet (HDD).

> [!IMPORTANT]
> * Om systemet har mer än en felande disken, ta inte bort mer än en SSD och HDD från hello system när som helst i tid. Detta kan resultera i förlust av data.
> * Se till att du placerar en ersättning SSD i en plats som tidigare har innehållit en SSD. Placera en annan Hårddisk på samma sätt i en plats som tidigare har innehållit en Hårddisk.
> * I hello Azure-portalen, fack numreras från 0 – 11. Därför om hello portal visar att en disk i uttag 2 misslyckades på hello enheten, leta efter hello disken hello tredje plats från hello övre vänstra.
> 
> 

Enheter kan tas bort och ersätts medan hello system är igång.

#### <a name="tooremove-a-drive"></a>tooremove en enhet
1. tooidentify hello misslyckades disk i hello Azure-portalen gå tooyour enhet **Inställningar > maskinvara hälsa**. Eftersom en disk kan misslyckas i hello primära hölje och/eller i en EBOD hölje (om du använder en 8600-modellen), se hello status för hello diskar under **delade komponenter** och under **EBOD delade komponenter** . En disk som misslyckats i antingen hölje visas med röd status.
2. Leta upp hello enheter i hello framför hello primära hölje eller hello EBOD hölje. 
3. Om det är olåst hello disk kan fortsätta toohello nästa steg. Om hello disk är låst kan låsa upp genom att följa proceduren hello i [kopplas ur hello antitamper Lås](#disengage-the-antitamper-lock).
4. Tryck på hello svart lås på hello enhet operatör modulen och dra hello enhet operatör handtaget ut och direkt från hello framför hello chassi.
   
    ![Frigöra referensen diskenhet](./media/storsimple-disk-drive-replacement/IC741051.png)
   
    **Bild 3** frisläppning hello enhet referensen
5. När hello enhet operatör referensen är helt utsträckt bild hello enhet operatör utanför hello chassi. 
   
    ![Glidande disk diskenhet](./media/storsimple-disk-drive-replacement/IC741052.png)
   
    **Bild 4** glidande hello diskenhet utanför hello operatör

## <a name="install-hello-replacement-disk-drive"></a>Installera hello ersättning diskenhet
När en enhet har misslyckats i din StorSimple-enhet och har tagits bort, så den här proceduren tooreplace den med en ny enhet.

#### <a name="tooinsert-a-drive"></a>tooinsert en enhet
1. Se till att hello enhet operatör referensen utökas helt, enligt följande bild hello.
   
    ![Enhet med referensen utökad](./media/storsimple-disk-drive-replacement/IC741044.png)
   
    **Bild 5** enheten med referensen utökad
2. Skjut hello enhet operatör alla hello sätt i hello chassi.
   
    ![Glidande disk i enhet operatör](./media/storsimple-disk-drive-replacement/IC741045.png)
   
    **Bild 6** glidande hello enhet operatör i hello chassi
3. Med hello enhet operatör infogade, Stäng hello enhet operatör referensen när fortsätter toopush hello enhet operatör i hello chassi tills hello enhet operatör referensen fästs i låst läge.
4. Använd hello Lås nyckel som tillhandahölls av Microsoft (tamperproof Torx för) toosecure hello operatör referensen till rätt plats genom att aktivera hello Lås skruv Stäng för ett kvartal medurs.
5. Kontrollera att hello ersättning lyckades och hello enheten är i drift. Komma åt hello Azure-portalen och navigera för**inställningar** > **maskinvara hälsa**. Under **delade komponenter** eller **EBOD delade komponenter**, status för hello enheten ska vara grön, som anger att den är felfri.
<!---Loc Comment: It seems it should say "Device settings > Hardware health" instead of "Settings > Hardware health"---->
   
   > [!NOTE]
   > Det kan ta flera timmar hello disk status tooturn grön efter hello ersättning.
  
## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).

