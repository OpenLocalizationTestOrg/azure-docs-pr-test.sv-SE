---
title: "aaaInstall uppdatering 2 på StorSimple-enheten | Microsoft Docs"
description: "Förklarar hur tooinstall StorSimple 8000 Series uppdatering 2 på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 8c8981df-75d9-4d19-b137-d6c6ba39dcfb
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 09/21/2016
ms.author: alkohli
ms.openlocfilehash: 33a0bea4358c944644563192f686af332d2ad7bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-2-on-your-storsimple-device"></a>Installera uppdatering 2 på din StorSimple-enhet
## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooinstall uppdatering 2 på en StorSimple-enhet som kör en tidigare programvaruversion via hello klassiska Azure-portalen. hello kursen omfattar också hello steg som krävs för hello uppdateringen när en gateway har konfigurerats på ett nätverksgränssnitt än DATA 0 av hello StorSimple-enhet och du försöker tooupdate från före uppdateringen 1 programvaruversionen.

Uppdatering 2 innehåller enheten programuppdateringar, LSI drivrutinsuppdateringar och uppdateringar av inbyggd disk. hello enhetsprogrammet och LSI uppdateringar utan avbrott uppdateringar och kan tillämpas via hello klassiska Azure-portalen. uppdateringar av hello disk inbyggd störande uppdateringar och kan endast användas via hello Windows PowerShell-gränssnittet för hello enhet.

> [!IMPORTANT]
> * Du kan inte se uppdatering 2 direkt, eftersom vi gör en stegvis distribution av hello uppdateringar. Söker efter uppdateringar igen några dagar som den här uppdateringen blir tillgänglig snart.
> * En uppsättning manuella och automatiska före kontroller är klar tidigare toohello installera toodetermine hello enhetens hälsotillstånd vad gäller maskinvara tillstånd och nätverksanslutningen. Kontrollerna före utförs endast om du använder hello uppdateringar från hello klassiska Azure-portalen.
> * Vi rekommenderar att du installerar hello programvara och drivrutinsuppdateringar via hello klassiska Azure-portalen. Du bör bara gå toohello Windows PowerShell-gränssnittet för hello enhet (tooinstall uppdateringar) om hello före uppdateringen gateway kontrollen misslyckas i hello-portalen. hello uppdateringar kan ta 4 – 7 timmar tooinstall (inklusive hello Windows-uppdateringar). hello Underhåll läge uppdateringar måste installeras via hello Windows PowerShell-gränssnittet för hello enhet. Eftersom Underhåll läge uppdateringar är störande uppdateringar, kommer dessa nertid för din enhet.
> * Om körs hello valfria StorSimple Snapshot Manager, kontrollerar du att du har uppgraderat Snapshot Manager version tooUpdate 2 tidigare tooupdating hello enheten.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-2-via-hello-azure-classic-portal"></a>Installera uppdatering 2 via hello klassiska Azure-portalen
Utför följande steg tooupdate hello enheten för[uppdatering 2](storsimple-update2-release-notes.md).

> [!NOTE]
> 2 kan du använda Microsoft toopull ytterligare diagnostikinformation från hello enhet. Därför när våra driftteamet identifierar enheter som har problem, vi är bättre utrustade toocollect information från hello enhet och diagnostisera problem. Genom att acceptera uppdatering 2 du låta oss tooprovide proaktiv stödet.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Kontrollera att enheten kör **StorSimple 8000 Series uppdatering 2 (6.3.9600.17673)**. Hej **senast uppdaterad datum** bör också ändras. Du får också se att Underhåll läge uppdateringar är tillgängliga (det här meddelandet kan fortsätta toobe som visas för dig too24 hello timmar efter installation av uppdateringar).
   
   Underhåll läge uppdateringar är störande uppdateringar som leda till enheten driftstopp och kan endast användas via hello Windows PowerShell-gränssnittet på enheten. I vissa fall när du kör Update 1.2 den inbyggda programvaran disk kan redan vara uppdaterade, i så fall behöver du inte tooinstall uppdaterar alla underhållsläge.
2. Hämta hello Underhåll läge uppdateringar med hjälp av stegen i hello [toodownload snabbkorrigeringar](#to-download-hotfixes) toosearch för och hämta KB3121899 som installerar uppdateringar av inbyggd disk (hello andra uppdateringar ska vara installerad nu).
3. Följ stegen i hello [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello Underhåll läge uppdateringar.

## <a name="install-update-2-as-a-hotfix"></a>Installera uppdatering 2 som en snabbkorrigering
Använd följande procedur om du inte hello gateway kontroll när du försöker tooinstall hello uppdateringar via hello klassiska Azure-portalen. hello misslyckas när du har en gateway som tilldelats tooa utan DATA 0-nätverksgränssnittet och din enhet med en software version tidigare tooUpdate 1.

hello programvaruversioner som kan uppgraderas med hello snabbkorrigeringen metoden är uppdatering 0.1, uppdatering 0,2, och uppdatering 0.3, uppdatering 1, uppdatering 1.1 och uppdatera 1.2. hello snabbkorrigeringen metoden innebär hello följande tre steg:

* Hämta hello snabbkorrigeringar från hello Microsoft Update-katalogen.
* Installera och kontrollera hello standardläget snabbkorrigeringar.
* Installera och kontrollera hello Underhåll läge snabbkorrigering.

tooinstall uppdatering 2 som en snabbkorrigering som du måste hämta och installera följande snabbkorrigeringar hello:

| Ordning | kB | Beskrivning | Typ av uppdatering |
| --- | --- | --- | --- |
| 1 |KB3121901 |Programuppdatering |Vanliga |
| 2 |KB3121900 |LSI drivrutin |Vanliga |
| 3 |KB3080728 |Storport-korrigering </br> Windows Server 2012 R2 |Vanliga |
| 4 |KB3090322 |Spaceport korrigering </br> Windows Server 2012 R2 |Vanliga |
| 5 |KB3121899 |Disk inbyggd programvara |Underhåll |

> [!IMPORTANT]
> * Om enheten kör versionen (GA) version, kontakta [Microsoft-supporten](storsimple-contact-microsoft-support.md) tooassist du med hello uppdatera.
> * Den här proceduren måste toobe utföras en gång tooapply uppdatering 2. Du kan använda hello Azure klassiska portal tooapply efterföljande uppdateringar.
> * Varje snabbkorrigeringsinstallationen kan ta cirka 20 minuter toocomplete. Totalt antal installationstid är nära too2 timmar.
> * Innan du använder den här proceduren tooapply hello uppdatering, kontrollera att båda styrenheter är online och att alla hello maskinvarukomponenter är felfria.
> 
> 

Utför följande steg tooapply hello uppdateringen som en snabbkorrigering.

[!INCLUDE [storsimple-install-update2-hotfix](../../includes/storsimple-install-update2-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om hello [uppdatering 2 versionen](storsimple-update2-release-notes.md).

