---
title: aaaReplace en StorSimple 8600 EBOD styrenhet | Microsoft Docs
description: "Förklarar hur tooremove och Ersätt en eller båda EBOD domänkontrollanter på en StorSimple 8600-enhet."
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
ms.date: 06/02/2017
ms.author: alkohli
ms.openlocfilehash: 8343ed6f48ae97fc9204452f85e1936bfb1d6919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-an-ebod-controller-on-your-storsimple-device"></a>Ersätta en domänkontrollant för en EBOD på din StorSimple-enhet

## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooreplace en felaktig EBOD domänkontrollant modul på Microsoft Azure StorSimple-enheten. tooreplace en EBOD Styrenhetsmodul, måste du:

* Ta bort hello felaktiga EBOD domänkontrollant
* Installera en ny domänkontrollant EBOD

Överväg följande information innan du börjar hello:

* Tom EBOD moduler måste infogas i alla oanvända platser. hello höljet ska inte kall korrekt om en plats är öppna.
* Hej EBOD domänkontrollanten är växlas och kan tas bort eller ersättas. Ta inte bort en misslyckad modul tills du har en ersättning. När du initierar hello ersättning process, måste du slutföra det inom 10 minuter.

> [!IMPORTANT]
> Innan du försöker tooremove eller ersätta en StorSimple-komponent, se till att du granskar hello [säkerhet ikonen konventioner](storsimple-safety.md#safety-icon-conventions) och andra [säkerhetsåtgärderna](storsimple-safety.md).

## <a name="remove-an-ebod-controller"></a>Ta bort en domänkontrollant för en EBOD
Ersätta hello misslyckades EBOD domänkontrollant modul i din StorSimple-enhet, kontrollera att den hello andra EBOD domänkontrollant modulen är aktiv och körs. hello förklarar följande procedur och tabell hur tooremove hello EBOD domänkontrollant modulen.

#### <a name="tooremove-an-ebod-module"></a>tooremove en EBOD-modul
1. Öppna hello Azure-portalen.
2. Gå tooyour enhet och gå för**inställningar** > **maskinvara hälsa**, och kontrollerar att hello status för hello Indikator för hello aktiva EBOD domänkontrollant modulen är grönt och hello Indikator för hello misslyckades EBOD domänkontrollant modul är röda.
3. Leta upp hello misslyckades EBOD domänkontrollant modulen på hello baksidan hello enhet.
4. Ta bort hello kablar som ansluter hello EBOD domänkontrollant modulen toohello domänkontrollant innan du tar hello EBOD modulen utanför hello system.
5. Anteckna hello exakt SAS-port för hello EBOD domänkontrollant modul som var anslutna toohello domänkontrollant. När du ersätter hello EBOD modulen kommer du att nödvändiga toorestore hello toothis systemkonfigurationen.
   
   > [!NOTE]
   > Normalt Port A, som är märkta som blir **värd i** i hello följande diagram.
   
    ![Bakplan EBOD domänkontrollant](./media/storsimple-ebod-controller-replacement/IC741049.png)
   
     **Bild 1** tillbaka of EBOD modul
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Fel Indikator |
   | 2 |Indikator för ström |
   | 3 |SAS-anslutningar |
   | 4 |SAS-indikatorer |
   | 5 |Serieportar factory endast för användning |
   | 6 |Port en (värd i) |
   | 7 |Port B (Host ut) |
   | 8 |Port C (endast Factory användning) |

## <a name="install-a-new-ebod-controller"></a>Installera en ny domänkontrollant EBOD
hello följande procedur och tabell förklarar hur tooinstall en EBOD Styrenhetsmodul i din StorSimple-enhet.

#### <a name="tooinstall-an-ebod-controller"></a>tooinstall EBOD-styrenhet
1. Kontrollera hello EBOD enheten skador, särskilt toohello gränssnittet connector. Installera inte hello nya EBOD styrenhet om någon PIN-koder är böjda.
2. Öppna position, bild hello-modulen till hello höljet tills hello Lås interagera med hello Lås i hello.
   
    ![Installera EBOD domänkontrollant](./media/storsimple-ebod-controller-replacement/IC741050.png)
   
    **Bild 2** installerar hello EBOD domänkontrollant modul
3. Stäng hello spärren. Du bör höra en klickning som snabbt tillkallar hello spärren.
   
    ![Släppa EBOD spärren](./media/storsimple-ebod-controller-replacement/IC741047.png)
   
    **Bild 3** stänger hello EBOD modulen spärren
4. Återanslut hello-kablar. Använd hello exakta konfigurationen som fanns innan hello ersättning. Se följande diagram hello och för information om hur tooconnect hello kablar.
   
    ![Kabelansluta den 4U för ström](./media/storsimple-ebod-controller-replacement/IC770723.png)
   
    **Bild 4**. Ansluta kablar
   
   | Etikett | Beskrivning |
   |:--- |:--- |
   | 1 |Primär enhet |
   | 2 |PCM 0 |
   | 3 |PCM 1 |
   | 4 |Styrenhet 0 |
   | 5 |Kontrollant 1 |
   | 6 |EBOD styrenhet 0 |
   | 7 |EBOD kontrollant 1 |
   | 8 |EBOD hölje |
   | 9 |Kraftfördelningsenheter |

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-8000-hardware-component-replacement.md).

