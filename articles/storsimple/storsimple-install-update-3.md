---
title: "aaaInstall uppdatering 3 på StorSimple-enheten | Microsoft Docs"
description: "Förklarar hur tooinstall StorSimple 8000 Series uppdatering 3 på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: c6c4634d-4f3a-4bc4-b307-a22bf18664e1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a156b8919639f1c7afb0fdef3d882d40d48f1c48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-3-on-your-storsimple-8000-series-device"></a>Installera uppdatering 3 på enheten StorSimple 8000-serien

## <a name="overview"></a>Översikt

Den här självstudiekursen beskrivs hur tooinstall uppdatering 3 på en StorSimple-enhet som kör en tidigare programvaruversion via hello klassiska Azure-portalen och använder hello snabbkorrigeringen-metoden. hello snabbkorrigeringen metoden används när en gateway har konfigurerats på ett nätverksgränssnitt än DATA 0 av hello StorSimple-enhet och du försöker tooupdate från före uppdateringen 1 programvaruversionen.

Uppdatering 3 innehåller enhetsprogrammet, LSI drivrutiner och inbyggd programvara, Storport och Spaceport uppdateras. Om uppdaterar från uppdatering 2 eller en tidigare version du också vara nödvändiga tooapply iSCSI, WMI, och i vissa fall disk uppdateringar av inbyggd programvara. Hej enhetsprogrammet, WMI, iSCSI, LSI drivrutin, Spaceport och Storport korrigeringar uppdateras utan avbrott och kan tillämpas via hello klassiska Azure-portalen. uppdateringar av hello disk inbyggd störande uppdateringar och kan endast användas via hello Windows PowerShell-gränssnittet för hello enhet. 

> [!IMPORTANT]
> * En uppsättning manuella och automatiska före kontroller är klar tidigare toohello installera toodetermine hello enhetens hälsotillstånd vad gäller maskinvara tillstånd och nätverksanslutningen. Kontrollerna före utförs endast om du använder hello uppdateringar från hello klassiska Azure-portalen.
> * Vi rekommenderar att du installerar hello programvara och drivrutinsuppdateringar via hello klassiska Azure-portalen. Du bör bara gå toohello Windows PowerShell-gränssnittet för hello enhet (tooinstall uppdateringar) om hello före uppdateringen gateway kontrollen misslyckas i hello-portalen. Beroende på hello-version som du uppdaterar från kan hello uppdateringarna ta 1.5 2,5 timmar tooinstall. hello Underhåll läge uppdateringar måste installeras via hello Windows PowerShell-gränssnittet för hello enhet. Eftersom Underhåll läge uppdateringar är störande uppdateringar, kommer dessa nertid för din enhet.
> * Om körs hello valfria StorSimple Snapshot Manager, kontrollerar du att du har uppgraderat Snapshot Manager version tooUpdate 2 tidigare tooupdating hello enheten.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-3-via-hello-azure-classic-portal"></a>Installera uppdatering 3 via hello klassiska Azure-portalen
Utför följande steg tooupdate hello enheten för[uppdatering 3](storsimple-update3-release-notes.md).

> [!NOTE]
> Om du kopplar uppdatering 2 eller senare (inklusive uppdatering 2.1), kommer Microsoft att kan toopull ytterligare diagnostikinformation från hello enhet. Därför när våra driftteamet identifierar enheter som har problem, vi är bättre utrustade toocollect information från hello enhet och diagnostisera problem. Genom att acceptera uppdatering 2 eller senare, du kan vi tooprovide proaktiv stödet.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

Kontrollera att enheten kör **StorSimple 8000 Series uppdatering 3 (6.3.9600.17759)**. Hej **senast uppdaterad datum** bör också ändras. 
   - Om du uppdaterar från en tidigare version tooUpdate 2 kan även se att hello Underhåll läge uppdateringar är tillgängliga (det här meddelandet kan fortsätta toobe som visas för dig too24 hello timmar efter installation av uppdateringar).
     Underhåll läge uppdateringar är störande uppdateringar som leda till enheten driftstopp och kan endast användas via hello Windows PowerShell-gränssnittet på enheten. I vissa fall när du kör Update 1.2 den inbyggda programvaran disk kan redan vara uppdaterade, i så fall behöver du inte tooinstall uppdaterar alla underhållsläge.
   - Om du uppdaterar från uppdatering 2 eller senare, bör enheten nu uppdaterade. Du kan hoppa över hello nästa steg.

Hämta hello Underhåll läge uppdateringar med hjälp av stegen i hello [toodownload snabbkorrigeringar](#to-download-hotfixes) toosearch för och hämta KB3121899 som installerar uppdateringar av inbyggd disk (hello andra uppdateringar ska vara installerad nu). Följ stegen i hello [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello Underhåll läge uppdateringar. 

## <a name="install-update-3-as-a-hotfix"></a>Installera uppdatering 3 som en snabbkorrigering
Använd följande procedur om du inte hello gateway kontroll när du försöker tooinstall hello uppdateringar via hello klassiska Azure-portalen. hello misslyckas när du har en gateway som tilldelats tooa utan DATA 0-nätverksgränssnittet och din enhet med en software version tidigare tooUpdate 1.

hello programvaruversioner som kan uppgraderas med hello snabbkorrigeringen metoden är:

* Uppdatera 0.1, 0,2, 0,3
* Uppdatera 1, 1.1, 1.2
* Uppdatera 2, 2.1, 2.2 

> [!IMPORTANT]
> * Om enheten kör versionen (GA) version, kontakta [Microsoft-supporten](storsimple-contact-microsoft-support.md) tooassist du med hello uppdatera.
> 
> 

hello snabbkorrigeringen metoden innebär hello följande tre steg:

1. Hämta hello snabbkorrigeringar från hello Microsoft Update-katalogen.
2. Installera och kontrollera hello standardläget snabbkorrigeringar.
3. Installera och kontrollera hello Underhåll läge snabbkorrigering (endast vid uppdatering från före uppdateringen 2 programvara).

#### <a name="download-updates-for-your-device"></a>Hämta uppdateringar för din enhet
**Om enheten kör uppdatering 2.1 eller 2.2**, måste du hämta och installera följande snabbkorrigeringar i hello föreskrivs ordning hello:

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 1. |KB3186843 |Programuppdaterings &#42; |Vanliga <br></br>Icke-störande |~ 45 minuter |
| 2. |KB3186859 |LSI drivrutiner och inbyggd programvara |Vanliga <br></br>Icke-störande |~ 20 minuter |
| 3. |KB3121261 |Storport och Spaceport korrigering </br> Windows Server 2012 R2 |Vanliga <br></br>Icke-störande |~ 20 minuter |

&#42;  *Observera att hello programuppdateringen består av två binära filer: enheten programuppdateringen inleds med `all-hcsmdssoftwareupdate` hello TIS och Mds-agent som inleds med `all-cismdsagentupdatebundle`. hello enheten programuppdateringen måste vara installerad före hello TIS och Mds Agent. Du måste också starta om hello aktiva styrenheten via hello `Restart-HcsController` cmdlet när du har tillämpat hello TIS och Mds agentuppdatering (och innan du tillämpar hello återstående uppdateringar).* 

**Om enheten kör uppdatering 0.1, 0,2, 0.3, 1.0, 1.1, 1.2 eller 2.0**, måste du hämta och installera följande snabbkorrigeringar i tillägg toohello programvara, LSI drivrutinen hello och inbyggd programvara uppdaterar (visas i hello föregående tabell), i hello föreskrivs ordning:

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 4. |KB3146621 |iSCSI-paket |Vanliga <br></br>Icke-störande |~ 20 minuter |
| 5. |KB3103616 |WMI-paket |Vanliga <br></br>Icke-störande |~ 12 minuter |

<br></br>

**Om enheten körs versioner 0,2, 0.3, 1.0, 1.1 och 1.2**, du måste även uppdateringar av tooinstall disk inbyggd ovanpå alla hello uppdateringar visas i hello föregående tabellerna. Du kan kontrollera om du behöver hello uppdateringar av inbyggd disk genom att köra hello `Get-HcsFirmwareVersion` cmdlet. Om du kör dessa versioner av inbyggd: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, och du inte behöver tooinstall uppdateringarna.

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 6. |KB3121899 |Disk inbyggd programvara |Underhåll <br></br>Störande |~ 30 minuter |

<br></br>

> [!IMPORTANT]
> * Den här proceduren måste toobe utföras en gång tooapply uppdatering 3. Du kan använda hello Azure klassiska portal tooapply efterföljande uppdateringar.
> * Om du uppdaterar från uppdateringen 2.2 är hello totala installationstid Stäng too1.1 timmar.
> * Innan du använder den här proceduren tooapply hello uppdatering, kontrollera att båda hello-styrenheter är online och alla hello maskinvarukomponenter är felfria.
> 
> 

Utför följande steg toodownload hello och installera hello snabbkorrigeringar.

[!INCLUDE [storsimple-install-update3-hotfix](../../includes/storsimple-install-update3-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om hello [uppdatering 3 versionen](storsimple-update3-release-notes.md).

