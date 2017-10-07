---
title: "aaaInstall uppdatering 4 på StorSimple-enheten | Microsoft Docs"
description: "Förklarar hur tooinstall StorSimple 8000 Series uppdatering 4 på enheten StorSimple 8000-serien."
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
ms.date: 05/30/2017
ms.author: alkohli
ms.openlocfilehash: 62c0ae94afdbb1027d3075962afa04d49fd1f60a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-4-on-your-storsimple-device"></a>Installera uppdatering 4 på din StorSimple-enhet

## <a name="overview"></a>Översikt

Den här självstudiekursen beskrivs hur tooinstall uppdatering 4 på en StorSimple-enhet som kör en tidigare programvaruversion via hello klassiska Azure-portalen och använder hello snabbkorrigeringen-metoden. hello snabbkorrigeringen metoden används när en gateway har konfigurerats på ett nätverksgränssnitt än DATA 0 av hello StorSimple-enhet och du försöker tooupdate från före uppdateringen 1 programvaruversionen.

Uppdatering 4 innehåller enhetsprogrammet, över inbyggd programvara, LSI drivrutiner och inbyggd programvara, Storport och Spaceport, OS säkerhetsuppdateringar och en mängd andra OS-uppdateringar.  hello enhetsprogrammet, över inbyggd programvara, Spaceport, Storport och andra uppdateringar av OS uppdateras utan avbrott. hello utan avbrott eller regelbundna uppdateringar kan tillämpas via hello klassiska Azure-portalen eller via hello snabbkorrigeringen metod. uppdateringar av hello disk inbyggd störande uppdateringar och kan endast användas via hello snabbkorrigeringen metoden hello Windows PowerShell-gränssnittet för hello enhet. 

> [!IMPORTANT]
> * En uppsättning manuella och automatiska före kontroller är klar tidigare toohello installera toodetermine hello enhetens hälsotillstånd vad gäller maskinvara tillstånd och nätverksanslutningen. Kontrollerna före utförs endast om du använder hello uppdateringar från hello klassiska Azure-portalen.
> * Vi rekommenderar att du installerar hello programvara och andra regelbundna uppdateringar via hello klassiska Azure-portalen. Du bör bara gå toohello Windows PowerShell-gränssnittet för hello enhet (tooinstall uppdateringar) om hello före uppdateringen gateway kontrollen misslyckas i hello-portalen. Beroende på hello-version som du uppdaterar från hello uppdateringarna kan ta 4 timmar (eller högre) tooinstall. hello Underhåll läge uppdateringar installeras även via hello Windows PowerShell-gränssnittet för hello enhet. Eftersom Underhåll läge uppdateringar är störande uppdateringar, kommer dessa nertid för din enhet.
> * Om körs hello valfria StorSimple Snapshot Manager, kontrollerar du att du har uppgraderat Snapshot Manager version tooUpdate 4 tidigare tooupdating hello enheten.


[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-4-via-hello-azure-classic-portal"></a>Installera uppdatering 4 via hello klassiska Azure-portalen
Utför följande steg tooupdate hello enheten för[uppdatering 4](storsimple-update4-release-notes.md).

> [!NOTE]
> Om du kopplar uppdatering 2 eller senare (inklusive uppdatering 2.1), kommer Microsoft att kan toopull ytterligare diagnostikinformation från hello enhet. Därför när våra driftteamet identifierar enheter som har problem, vi är bättre utrustade toocollect information från hello enhet och diagnostisera problem. Genom att acceptera uppdatering 2 eller senare, du kan vi tooprovide proaktiv stödet. 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Kontrollera att enheten kör **StorSimple 8000 Series uppdatering 4 (6.3.9600.17820)**. Hej **senast uppdaterad datum** bör också ändras. 

* Du ser nu att hello Underhåll läge uppdateringar är tillgängliga (det här meddelandet kan fortsätta toobe som visas för dig too24 hello timmar efter installation av uppdateringar). Underhåll läge uppdateringar är störande uppdateringar som leda till enheten driftstopp och kan endast användas via hello Windows PowerShell-gränssnittet på enheten.
 
* Hämta hello Underhåll läge uppdateringar med hjälp av stegen i hello [toodownload snabbkorrigeringar](#to-download-hotfixes) toosearch för och hämta KB4011837 som installerar uppdateringar av inbyggd disk (hello andra uppdateringar ska vara installerad nu). Följ stegen i hello [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello Underhåll läge uppdateringar. 

## <a name="install-update-4-as-a-hotfix"></a>Installera uppdatering 4 som en snabbkorrigering
hello bör metoden tooinstall uppdatering 4 är via hello klassiska Azure-portalen.

Använd följande procedur om du inte hello gateway kontroll när du försöker tooinstall hello uppdateringar via hello klassiska Azure-portalen. hello misslyckas när du har en gateway som tilldelats tooa utan DATA 0-nätverksgränssnittet och din enhet med en software version tidigare tooUpdate 1.

hello programvaruversioner som kan uppgraderas med hello snabbkorrigeringen metoden är:

* Uppdatera 0.1, 0,2, 0,3
* Uppdatera 1, 1.1, 1.2
* Uppdatera 2, 2.1, 2.2
* Uppdatering 3, 3.1 


hello snabbkorrigeringen metoden innebär hello följande tre steg:

1. Hämta hello snabbkorrigeringar från hello Microsoft Update-katalogen.
2. Installera och kontrollera hello standardläget snabbkorrigeringar.
3. Installera och kontrollera hello Underhåll läge snabbkorrigering.

#### <a name="download-updates-for-your-device"></a>Hämta uppdateringar för din enhet

Du måste hämta och installera hello följande snabbkorrigeringar i hello föreskrivs ordning och hello föreslagna mappar:

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |Installera i mappen|
| --- | --- | --- | --- | --- | --- |
| 1. |KB4011839 <br> (2-filer) |Programuppdateringen för enhet <br> TIS/MDS agentuppdatering |Vanliga <br></br>Icke-störande |~ 25 minuter |FirstOrderUpdate <br> _Installera enheten programuppdatering före uppdateringen av TIS/MDS-agent_|
| 2A. |KB4011841 <br> KB4011842 |LSI drivrutiner och uppdateringar av inbyggd programvara <br> ÖVER firmware-uppdatering (version 3.38) |Vanliga <br></br>Icke-störande |~ 3 timmar <br> (inklusive 2A. + 2B. + 2 C.)|SecondOrderUpdate|
| 2B. |KB3139398 KB3108381 <br> KB3205400 KB3142030 <br> KB3197873 KB3192392  <br> KB3153704 KB3174644 <br> KB3139914  |Uppdateringar av OS säkerhetspaketet |Vanliga <br></br>Icke-störande |- |SecondOrderUpdate|
| 2C. |KB3210083 KB3103616 <br> KB3146621 KB3121261 <br> KB3123538 |Paketet för OS-uppdateringar |Vanliga <br></br>Icke-störande |- |SecondOrderUpdate|

Du kanske också måste tooinstall disk programvara på alla hello uppdateringar visas i hello föregående tabellerna. Du kan kontrollera om du behöver hello uppdateringar av inbyggd disk genom att köra hello `Get-HcsFirmwareVersion` cmdlet. Om du kör dessa versioner av inbyggd: `XMGJ`, `XGEG`, `KZ50`, `F6C2`, `VR08`, `N002`, `0106`, och du inte behöver tooinstall uppdateringarna.

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid | Installera i mappen|
| --- | --- | --- | --- | --- | --- |
| 3. |KB4011837 |Disk inbyggd programvara |Underhåll <br></br>Störande |~ 30 minuter | ThirdOrderUpdate |

<br></br>

> [!IMPORTANT]
> * Den här proceduren måste toobe utföras en gång tooapply uppdatering 4. Du kan använda hello Azure klassiska portal tooapply efterföljande uppdateringar.
> * Om uppdaterar från uppdatering 3 eller 3.1 är hello totala installationstid Stäng too4 timmar.
> * Innan du använder den här proceduren tooapply hello uppdatering, kontrollera att båda hello-styrenheter är online och alla hello maskinvarukomponenter är felfria.

Utför följande steg toodownload hello och installera hello snabbkorrigeringar.

[!INCLUDE [storsimple-install-update4-hotfix](../../includes/storsimple-install-update4-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om hello [uppdatering 4 versionen](storsimple-update4-release-notes.md).

