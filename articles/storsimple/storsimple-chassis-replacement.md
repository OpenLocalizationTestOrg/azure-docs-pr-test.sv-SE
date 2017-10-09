---
title: "aaaReplace chassi på StorSimple 8000-serieenhet | Microsoft Docs"
description: "Beskriver hur tooremove och Ersätt hello chassi för din StorSimple primära hölje eller EBOD hölje."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 537659ed-4c46-49c1-b1e4-186262f2542d
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: f8576d63520a6f7d3267180d2a68d4fc38fd48fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replace-hello-chassis-on-your-storsimple-device"></a>Ersätt hello chassi på din StorSimple-enhet
## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooremove och ersätta ett chassi i en enhet med StorSimple 8000-serien. Hej StorSimple 8100 modellen är en enda enhet enhet (samma chassi) hello 8600 är en dubbel höljet enhet (två chassi). För en modell av 8600 är potentiellt två chassi som kan misslyckas på hello enhet: hello chassi för hello primära hölje eller hello chassi för hello EBOD hölje.

I båda fallen är hello ersättning chassi som levereras av Microsoft tom. Ingen ström och kylning moduler (PCMs) domänkontrollant moduler solid state disk-hårddiskar (SSD), hårddiskar (HDD) eller EBOD moduler tas med.

> [!IMPORTANT]
> Innan du tar bort och ersätter chassi hello, granska hello säkerhetsinformation i [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).
> 
> 

## <a name="remove-hello-chassis"></a>Ta bort hello chassi
Utföra hello följa steg tooremove hello chassi på din StorSimple-enhet.

#### <a name="tooremove-a-chassis"></a>tooremove ett chassi
1. Kontrollera att hello StorSimple-enheten stängs av och kopplas bort från alla hello strömkällor.
2. Ta bort alla hello nätverks- och SAS-kablar, om tillämpligt.
3. Ta bort hello enheten från hello rack.
4. Ta bort alla hello enheter och Observera hello fack de tas bort. Mer information finns i [ta bort hello diskenhet](storsimple-disk-drive-replacement.md#remove-the-disk-drive).
5. Ta bort hello EBOD domänkontrollant moduler på hello EBOD hölje (om detta är hello chassi som misslyckades). Mer information finns i [ta bort en domänkontrollant för en EBOD](storsimple-ebod-controller-replacement.md#remove-an-ebod-controller). 
   
    På hello primära hölje (om detta är hello chassi som misslyckades) ta bort hello-styrenheter och notera hello fack de tas bort. Mer information finns i [ta bort en domänkontrollant](storsimple-controller-replacement.md#remove-a-controller).

## <a name="install-hello-chassis"></a>Installera hello chassi
Utföra hello följa steg tooinstall hello chassi på din StorSimple-enhet.

#### <a name="tooinstall-a-chassis"></a>tooinstall ett chassi
1. Montera hello chassi i hello rack. Mer information finns i [rackmontera StorSimple-8100-enhet](storsimple-8100-hardware-installation.md#rack-mount-your-storsimple-8100-device) eller [rackmonterade StorSimple-8600-enhet](storsimple-8600-hardware-installation.md#rack-mount-your-storsimple-8600-device).
2. När hello chassi har monterats i hello rack, installerar du hello controller moduler i hello samma placerar att de tidigare har installerats i.
3. Installera hello enheter i hello samma placerar och fack som de installerades tidigare i.
   
   > [!NOTE]
   > Vi rekommenderar att du först installera hello SSD i hello platserna och sedan installera hello hårddiskar.
   > 
   > 
4. Med hello enheten monteras i hello rack och hello-komponenter, ansluta din enhet toohello lämplig strömkällor och aktivera hello enheten. Mer information finns i [Kabelanslut din 8100 StorSimple-enhet](storsimple-8100-hardware-installation.md#cable-your-storsimple-8100-device) eller [Kabelanslut din 8600 StorSimple-enhet](storsimple-8600-hardware-installation.md#cable-your-storsimple-8600-device).

## <a name="next-steps"></a>Nästa steg
Lär dig mer om [ersättning av StorSimple maskinvara komponenten](storsimple-hardware-component-replacement.md).

