---
title: aaaStorSimple 8000-serien uppdatering 5 viktig information | Microsoft Docs
description: "Beskriver nya funktioner för hello, problem och lösningar för StorSimple 8000 Series uppdatering 5."
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
ms.date: 08/23/2017
ms.author: alkohli
ms.openlocfilehash: 9eb8ffb97b41ce3d4f1ffdf2975f904d0a2958e7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-8000-series-update-5-release-notes"></a>StorSimple 8000 Series uppdatering 5 viktig information

## <a name="overview"></a>Översikt

hello följande viktig information beskrivs hello nya funktioner och identifiera hello kritiska öppna problem för StorSimple 8000 Series uppdatering 5. De innehåller också en lista över hello StorSimple programuppdateringar som ingår i den här versionen.

Uppdatering 5 kan vara tillämpade tooany StorSimple-enhet som kör uppdatering 0.1 med Update 4. hello enheten version som är associerad med uppdatering 5 är 6.3.9600.17845.

Granska hello informationen i hello release notes innan du distribuerar hello uppdatera i din StorSimple-lösning.

> [!IMPORTANT]
> * Uppdatering 5 har enhetsprogrammet, disk inbyggd programvara, OS-säkerhet och andra uppdateringar av OS. Det tar cirka 4 timmar tooinstall uppdateringen. Disk firmware-uppdatering är en störande uppdatering och resulterar i ett driftstopp för din enhet. Vi rekommenderar att du installerar uppdatering 5 tookeep enheten är uppdaterad.
> * För nya versioner, kan du inte se uppdateringar omedelbart eftersom vi gör en stegvis distribution av hello uppdateringar. Vänta några dagar och sedan söka efter uppdateringar som de här uppdateringarna igen är snart tillgängligt.

## <a name="whats-new-in-update-5"></a>Vad är nytt i uppdatering 5

hello har följande viktiga förbättringar och felkorrigeringar gjorts i uppdatering 5.

* **Användning av Azure Active Directory (AAD) tooauthenticate med StorSimple Enhetshanteraren tjänsten** – från uppdatering 5 och senare, Azure Active Directory är används tooauthenticate med hello StorSimple Device Manager-tjänsten. hello gamla autentiseringsmekanism att bli inaktuell med December 2017. Alla hello användarna måste inkludera hello nya autentisering URL: er i sina brandväggsregler. Mer information finns för[autentisering webbadresserna i hello nätverkskraven för din StorSimple-enhet](storsimple-8000-system-requirements.md#url-patterns-for-azure-portal).

    Om autentiserings-URL för hello inte ingår i hello brandväggsregler, ser hello användarna en kritisk varning som sin StorSimple-enhet inte kunde autentisera med hello-tjänsten. Om hello användarna ser den här aviseringen, måste de tooinclude hello nya autentiserings-URL. Mer information finns för[StorSimple nätverk aviseringar](storsimple-8000-manage-alerts.md#networking-alerts).

* **Ny version av StorSimple Snapshot Manager** -en ny version av StorSimple Snapshot Manager släpps med uppdatering 5. Vi rekommenderar att du uppdaterar toothis version. Den här versionen är kompatibel med alla hello StorSimple-enheter som kör uppdatering 3 eller senare. Mer information finns för[distribuera StorSimple Snapshot Manager](storsimple-snapshot-manager-deployment.md).


## <a name="issues-fixed-in-update-5"></a>Problem som åtgärdas i uppdatering 5

hello innehåller följande tabell en översikt över problem som har åtgärdats i uppdatering 5.

| Nej | Funktion | Problem | Gäller toophysical enhet | Gäller toovirtual enhet |
| --- | --- | --- | --- | --- |
| 1 |Windows PowerShell-fjärrkommunikation |I hello tidigare version, skulle en användare får ett felmeddelande när tooestablish en anslutning till toohello StorSimple moln enheten via Windows PowerShell. Det här problemet var roten orsakade och fast i den här versionen. |Nej |Ja |
| 2 |Bandbredd-mallar |Det uppstod ett problem med bandbredden mallar som resulterade i mindre bandbredd än vilken hello-enhet har konfigurerats för i tidigare version. Det här problemet är löst i den här versionen. |Ja |Ja |
| 3 |Redundans |I tidigare version, när en enhet med ett stort antal volymer gick över tooanother enhet som kör uppdatering 4 misslyckas hello processen vid försök tooapply hello access control-poster. Det här problemet löses i den här versionen. |Ja |Ja |



## <a name="known-issues-in-update-5-from-previous-releases"></a>Kända problem i uppdatering 5 från tidigare versioner

Det finns inga nya kända problem i uppdatering 5. En lista över problem som överförs från tidigare versioner tooUpdate 5 gå för[uppdatering 3 viktig information](storsimple-update3-release-notes.md#known-issues-in-update-3).

## <a name="serial-attached-scsi-sas-controller-and-firmware-updates-in-update-5"></a>Serieansluten SCSI (SAS) domänkontrollant och firmware-uppdateringar i uppdatering 5

Den här versionen har SAS-styrenhet och LSI drivrutinen och firmware-uppdateringar. Mer information om hur tooinstall dessa uppdateringar, se [installera uppdatering 5](storsimple-8000-install-update-5.md) på StorSimple-enheten.

## <a name="storsimple-cloud-appliance-updates-in-update-5"></a>StorSimple-enhet för molnet uppdateringar i uppdatering 5

Den här uppdateringen kan inte vara tillämpade toohello StorSimple moln installation (även kallat hello virtuell enhet). Nya installationer av molnet måste toobe som skapats med hjälp av hello uppdatering 5 avbildning. Mer information om hur toocreate en molnet StorSimple-enhet går för[distribuera och hantera en StorSimple-enhet för molnet](storsimple-8000-cloud-appliance-u2.md).

## <a name="next-step"></a>Nästa steg

Lär dig hur för[installera uppdatering 5](storsimple-8000-install-update-5.md) på StorSimple-enheten.

