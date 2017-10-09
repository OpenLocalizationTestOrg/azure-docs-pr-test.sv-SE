---
title: "aaaReplace batteri på Microsoft Azure StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooremove, Ersätt och underhålla hello säkerhetskopiering batteri modul på din StorSimple-enhet."
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
ms.date: 06/05/2017
ms.author: alkohli
ms.openlocfilehash: 5ac767807e6c3fd817d8d522629db2aceaac9bdf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-backup-battery-module-on-your-storsimple-device"></a>Ersätt hello säkerhetskopiering batteri modul på din StorSimple-enhet

## <a name="overview"></a>Översikt
hello primära höljet ström och kylning modul (PCM) på din Microsoft Azure StorSimple-enhet har en extra batteri pack. Paketet innehåller power så att hello StorSimple-enhet kan spara data om förlust av AC power toohello primära enhet. Den här batteri pack är refererad tooas hello *säkerhetskopiering batterimodulen*. hello säkerhetskopiering batteri modul finns bara för hello primära höljet i din StorSimple-enhet (hello EBOD hölje innehåller inte en säkerhetskopiering batteri modul).

Den här självstudiekursen beskrivs hur du:

* Ta bort hello säkerhetskopiering batteri modul
* Installera en ny modul som reserv batteri
* Underhåll hello säkerhetskopiering batteri modul

> [!IMPORTANT]
> Innan du tar bort och ersätter en reserv batteri modul, granska hello säkerhetsinformation i hello [ersättning av introduktion tooStorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).


## <a name="remove-hello-backup-battery-module"></a>Ta bort hello säkerhetskopiering batteri modul
hello säkerhetskopiering batteri modul för din StorSimple-enhet är en-fältutbytbar enhet. Innan det installeras i hello PCM ska hello batterimodulen lagras i dess ursprungliga förpackning. Utför följande steg tooremove hello säkerhetskopiering batteri hello.

#### <a name="tooremove-hello-backup-battery-module"></a>tooremove hello säkerhetskopiering batteri modul
1. Gå tooyour StorSimple Enhetshanteraren service bladet i hello Azure-portalen. Gå för**enheter** och väljer sedan enheten hello lista över enheter. Navigera för**övervakaren** > **maskinvara hälsa**. Under **delade komponenter**, se hello status för hello batteri.
2. Identifiera hello PCM i vilka hello batteri misslyckades. Bild 1 visar hello baksidan hello StorSimple-enhet.
   
    ![Bakplan av primära Höljesmoduler](./media/storsimple-battery-replacement/IC740994.png)
   
    **Bild 1** baksidan primära enhet visar PCM och controller moduler
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |PCM 0 |
   | 2 |PCM 1 |
   | 3 |Styrenhet 0 |
   | 4 |Kontrollant 1 |
   
    Enligt av talet 3 i hello bild 2 hello övervakning indikatorns LEDDE på PCM 0 som motsvarar för**batteri fel** bör upplysta.
   
    ![Bakplan av enheten PCM övervakning indikator indikatorer](./media/storsimple-battery-replacement/IC740992.png)
   
    **Bild 2** tillbaka of PCM visar hello övervakning indikatorer
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |AC strömavbrott |
   | 2 |Fläkt fel |
   | 3 |Batteri fel |
   | 4 |PCM OK |
   | 5 |DC strömavbrott |
   | 6 |Batteri felfri |
3. tooremove hello PCM med en misslyckad batteri åtgärderna hello i [ta bort en PCM](storsimple-power-cooling-module-replacement.md#remove-a-pcm).
4. Med hello PCM tas bort, lift och rotera hello batteri modulen hanterar uppåt som anges i följande bild hello och dra in tooremove hello batteri.
   
    ![Ta bort batteri från PCM](./media/storsimple-battery-replacement/IC741019.png)
   
    **Bild 3** tar bort hello batteri från hello PCM
5. Placera hello modul i hello-fältutbytbar enhet paketera.
6. Returnera hello felaktiga enheten tooMicrosoft för rätt underhåll och hantering.

## <a name="install-a-new-backup-battery-module"></a>Installera en ny modul som reserv batteri
Utföra hello följande steg tooinstall hello ersättning batteri modul i hello PCM i hello primära hölje av StorSimple-enheten.

#### <a name="tooinstall-hello-battery-module"></a>tooinstall hello batteri modul
1. Placera hello säkerhetskopiering batteri modul i hello rätt orientering i hello PCM.
2. Tryck ned hello batterimodulen hantera alla hello sätt tooseat hello connector.
3. Ersätt hello PCM i hello primära hölje genom att följa hello riktlinjerna i [ersätta en ström och kylning modulen på StorSimple-enheten](storsimple-power-cooling-module-replacement.md).
4. När hello ersättning är klar, går tooyour enhet och gå sedan för**övervakaren** > **maskinvara hälsa** i hello Azure-portalen. Kontrollera hello status för hello batteri toomake till att hello-installationen har lyckats. Grön status anger att hello batteri är felfri.

## <a name="maintain-hello-backup-battery-module"></a>Underhåll hello säkerhetskopiering batteri modul
I din StorSimple-enhet ger hello säkerhetskopiering batterimodulen power toohello styrenhet under ett Strömfel dataförlust. Hej StorSimple enheten toosave kritiska data tidigare tooshutting ned kan på ett kontrollerat sätt. Med två fullständigt debiteras batterierna i hello PCMs kan hello system hantera två på varandra följande förlorade händelser.

I hello Azure-portalen, hello **maskinvara hälsa** under hello **övervakaren** bladet anger om hello batteri är fel eller närmar sig hello slutet av livslängd. hello batteristatus anges med **batteri i PCM 0** eller **batteri i PCM 1** under **delade komponenter**. Det här bladet visar en **DEGRADERAD** tillstånd för det närmar sig slutet av livslängd, och **misslyckades** för nått end-för-livslängd.

> [!NOTE]
> hello batteri kan rapportera **misslyckades** när den behöver bara toobe debiteras.


Om hello **DEGRADERAD** status visas, rekommenderar vi hello följande åtgärd:

* hello system kan ha uppstått nyligen strömavbrott eller hello batterierna kan väldigt regelbundet underhåll. Se hello system för 12 timmar innan du fortsätter.
  
  * Om hello tillstånd är fortfarande **DEGRADERAD** efter 12 timmar kontinuerlig anslutning tooAC ström med hello domänkontrollanter och PCMs körs sedan hello batteri måste toobe ersättas. Kontrollera [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md) för en ersättning säkerhetskopiering batteri modul.
  * Om hello tillstånd blir OK efter 12 timmar, hello batteri fungerar och behövs endast en avgift för underhåll.
* Om det inte har en associerad förlust av AC-ström och hello PCM är påslagen och ansluten tooAC power, måste hello batteri toobe ersättas. [Kontakta Microsoft Support](storsimple-8000-contact-microsoft-support.md) tooorder en ersättning säkerhetskopiering batteri modul.

> [!IMPORTANT]
> Ta bort hello misslyckades batteri enligt toonational och nationella föreskrifter.

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).

