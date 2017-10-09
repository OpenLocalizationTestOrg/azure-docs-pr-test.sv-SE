---
title: "aaaInstall uppdatering 2.2 på StorSimple-enheten | Microsoft Docs"
description: "Förklarar hur tooinstall StorSimple 8000 Series uppdatering 2.2 på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 047c7a4b-73d0-45ea-8d51-c54d71871392
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/02/2016
ms.author: alkohli
ms.openlocfilehash: cedb83ce42bc6bb81a4e43345da3f25b71036d1a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-22-on-your-storsimple-device"></a>Installera uppdatering 2.2 på din StorSimple-enhet
## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooinstall uppdatering 2.2 på en StorSimple-enhet som kör en tidigare programvaruversion via hello klassiska Azure-portalen och använder hello snabbkorrigeringen-metoden. hello snabbkorrigeringen metoden används när en gateway har konfigurerats på ett nätverksgränssnitt än DATA 0 av hello StorSimple-enhet och du försöker tooupdate från före uppdateringen 1 programvaruversionen.

2.2 uppdateringen enhetsprogrammet, WMI- och iSCSI-uppdateringar. Om uppdatering från version 2.1 måste endast hello enheten programuppdateringen toobe tillämpas. Om uppdatering från en före uppdateringen 2 version kan vara du också nödvändiga tooapply LSI drivrutin, Spaceport, Storport och uppdateringar av inbyggd disk. Hej enhetsprogrammet, WMI, iSCSI, LSI drivrutin, Spaceport och Storport korrigeringar uppdateras utan avbrott och kan tillämpas via hello klassiska Azure-portalen. uppdateringar av hello disk inbyggd störande uppdateringar och kan endast användas via hello Windows PowerShell-gränssnittet för hello enhet. 

> [!IMPORTANT]
> * En uppsättning manuella och automatiska före kontroller är klar tidigare toohello installera toodetermine hello enhetens hälsotillstånd vad gäller maskinvara tillstånd och nätverksanslutningen. Kontrollerna före utförs endast om du använder hello uppdateringar från hello klassiska Azure-portalen.
> * Vi rekommenderar att du installerar hello programvara och drivrutinsuppdateringar via hello klassiska Azure-portalen. Du bör bara gå toohello Windows PowerShell-gränssnittet för hello enhet (tooinstall uppdateringar) om hello före uppdateringen gateway kontrollen misslyckas i hello-portalen. Beroende på hello-version som du uppdaterar från kan hello uppdateringarna ta 1.5 2,5 timmar tooinstall. hello Underhåll läge uppdateringar måste installeras via hello Windows PowerShell-gränssnittet för hello enhet. Eftersom Underhåll läge uppdateringar är störande uppdateringar, kommer dessa nertid för din enhet.
> * Om körs hello valfria StorSimple Snapshot Manager, kontrollerar du att du har uppgraderat Snapshot Manager version tooUpdate 2.2 tidigare tooupdating hello enheten.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-22-via-hello-azure-classic-portal"></a>Installera uppdatering 2.2 via hello klassiska Azure-portalen
Utför följande steg tooupdate hello enheten för[uppdatering 2.2](storsimple-update21-release-notes.md).

> [!NOTE]
> Om du kopplar uppdatering 2 eller senare (inklusive uppdatering 2.1), kommer Microsoft att kan toopull ytterligare diagnostikinformation från hello enhet. Därför när våra driftteamet identifierar enheter som har problem, vi är bättre utrustade toocollect information från hello enhet och diagnostisera problem. Genom att acceptera uppdatering 2 eller senare, du kan vi tooprovide proaktiv stödet.
> 
> 

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Kontrollera att enheten kör **StorSimple 8000 Series uppdatering 2.2 (6.3.9600.17708)**. Hej **senast uppdaterad datum** bör också ändras. 
   
   Om du uppdaterar från en tidigare version tooUpdate 2 kan även se att hello Underhåll läge uppdateringar är tillgängliga (det här meddelandet kan fortsätta toobe som visas för dig too24 hello timmar efter installation av uppdateringar).
   
   Underhåll läge uppdateringar är störande uppdateringar som leda till enheten driftstopp och kan endast användas via hello Windows PowerShell-gränssnittet på enheten. I vissa fall när du kör Update 1.2 den inbyggda programvaran disk kan redan vara uppdaterade, i så fall behöver du inte tooinstall uppdaterar alla underhållsläge.
   
   Om du uppdaterar från uppdatering 2 bör enheten nu uppdaterade. Du kan hoppa över hello återstående steg.
2. Hämta hello Underhåll läge uppdateringar med hjälp av stegen i hello [toodownload snabbkorrigeringar](#to-download-hotfixes) toosearch för och hämta KB3121899 som installerar uppdateringar av inbyggd disk (hello andra uppdateringar ska vara installerad nu).
3. Följ stegen i hello [installera och underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello Underhåll läge uppdateringar. 

## <a name="install-update-22-as-a-hotfix"></a>Installera uppdatering 2.2 som en snabbkorrigering
Använd följande procedur om du inte hello gateway kontroll när du försöker tooinstall hello uppdateringar via hello klassiska Azure-portalen. hello misslyckas när du har en gateway som tilldelats tooa utan DATA 0-nätverksgränssnittet och din enhet med en software version tidigare tooUpdate 1.

hello programvaruversioner som kan uppgraderas med hello snabbkorrigeringen metoden är:

* Uppdatera 0.1, 0,2, 0,3
* Uppdatera 1, 1.1, 1.2
* Uppdatering 2, 2.1 

> [!IMPORTANT]
> * Om enheten kör versionen (GA) version, kontakta [Microsoft-supporten](storsimple-contact-microsoft-support.md) tooassist du med hello uppdatera.
> 
> 

hello snabbkorrigeringen metoden innebär hello följande tre steg:

* Hämta hello snabbkorrigeringar från hello Microsoft Update-katalogen.
* Installera och kontrollera hello standardläget snabbkorrigeringar.
* Installera och kontrollera hello Underhåll läge snabbkorrigering (endast vid uppdatering från före uppdateringen 2 programvara).

#### <a name="download-updates-for-a-device-running-update-21-software"></a>Hämta uppdateringar för en enhet som kör uppdatering 2.1 programvara
**Om enheten kör uppdatera 2.1**, måste du hämta endast hello enheten programuppdateringen KB3179904. Installera endast hello-binärfilen som inleds med ”alla hcsmdssoftwareudpate'. Installera inte hello TIS och hello MDS agentuppdatering inleds med `all-cismdsagentupdatebundle`. Fel toodo så resulterar i ett fel. Detta är en icke-störande uppdatering, kommer inte att avbrytas i/o och hello enheten inte driftavbrott.

#### <a name="download-updates-for-a-device-running-update-2-software"></a>Hämta uppdateringar för en enhet som kör uppdatering 2
**Om enheten kör uppdatering 2**, måste du hämta och installera följande snabbkorrigeringar i hello föreskrivs ordning hello:

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 1. |KB3179904 |Programuppdaterings &#42; |Vanliga <br></br>Icke-störande |~ 45 minuter |
| 2. |KB3146621 |iSCSI-paket |Vanliga <br></br>Icke-störande |~ 20 minuter |
| 3. |KB3103616 |WMI-paket |Vanliga <br></br>Icke-störande |~ 12 minuter |

 &#42;  *Observera att programuppdateringen som består av två binära filer: enheten programuppdateringen inleds med `all-hcsmdssoftwareupdate` hello TIS och Mds-agent som inleds med `all-cismdsagentupdatebundle`. hello enheten programuppdateringen måste vara installerad före hello TIS och Mds-agenten. Du måste också starta om hello aktiva styrenheten via hello `Restart-HcsController` cmdlet när du har tillämpat hello TIS och MDS agentuppdatering (och innan du tillämpar hello återstående uppdateringar).* 

#### <a name="download-updates-for-a-device-running-pre-update-2-software"></a>Hämta uppdateringar för en enhet som körs före uppdatering 2 programvara
**Om enheten körs versioner 0,2, 0.3, 1.0 och 1.1**, måste du hämta och installera hello LSI drivrutiner och inbyggd programvara uppdatera dessutom toohello programvara, iSCSI och WMI-uppdateringar. Den här uppdateringen har redan installerats om du kör uppdatera 1.2 eller 2. 

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 4. |KB3121900 |LSI drivrutiner och inbyggd programvara |Vanliga <br></br>Icke-störande |~ 20 minuter |

<br></br>
**Om enheten körs versioner 0,2, 0.3, 1.0, 1.1 och 1.2**, måste du hämta och installera hello Spaceport och hello Storport korrigering. Dessa har redan installerats om du kör uppdatering 2.

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 5. |KB3090322 |Spaceport korrigering </br> Windows Server 2012 R2 |Vanliga <br></br>Icke-störande |~ 20 minuter |
| 6. |KB3080728 |Storport-korrigering </br> Windows Server 2012 R2 |Vanliga <br></br>Icke-störande |~ 20 minuter |

<br></br>
Du kanske också måste tooinstall disk firmware-uppdateringar. Du kan kontrollera om du behöver hello uppdateringar av inbyggd disk genom att köra hello `Get-HcsFirmwareVersion` cmdlet. Om du kör dessa versioner av inbyggd: `XMGG`, `XGEG`, `KZ50`, `F6C2`, `VR08`, och du inte behöver tooinstall uppdateringarna.

| Ordning | kB | Beskrivning | Typ av uppdatering | Installationstid |
| --- | --- | --- | --- | --- |
| 7. |KB3121899 |Disk inbyggd programvara |Underhåll <br></br>Störande |~ 30 minuter |

<br></br>

> [!IMPORTANT]
> * Den här proceduren måste toobe utföras en gång tooapply uppdatering 2.2. Du kan använda hello Azure klassiska portal tooapply efterföljande uppdateringar.
> * Om du uppdaterar från uppdatering 2 är hello totala installationstid Stäng too1.5 timmar.
> * Innan du använder den här proceduren tooapply hello uppdatering, kontrollera att båda hello-styrenheter är online och alla hello maskinvarukomponenter är felfria.
> 
> 

Utför följande steg toodownload hello och installera hello snabbkorrigeringar.

[!INCLUDE [storsimple-install-update21-hotfix](../../includes/storsimple-install-update21-hotfix.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om hello [uppdatering 2.1 versionen](storsimple-update21-release-notes.md).

