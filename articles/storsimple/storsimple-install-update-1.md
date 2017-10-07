---
title: "aaaInstall Update 1.2 på StorSimple-enheten | Microsoft Docs"
description: "Förklarar hur tooinstall StorSimple 8000 Series uppdatering 1.2 på enheten StorSimple 8000-serien."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 7a513923-eb77-4078-b0ab-f8e90183796a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0a7601dc0b1ce60eb854227243ecb02d6fb2c678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-update-12-on-your-storsimple-8000-series-device"></a>Installera uppdatering 1.2 på enheten StorSimple 8000-serien
## <a name="overview"></a>Översikt
Den här självstudiekursen beskrivs hur tooinstall uppdatera 1.2 på en StorSimple-enhet som kör en programvara version tidigare tooUpdate 1. hello kursen omfattar också hello ytterligare steg som krävs för hello uppdatering när en gateway har konfigurerats på ett nätverksgränssnitt än DATA 0 av hello StorSimple-enhet.

1.2 uppdateringen enheten programuppdateringar, LSI drivrutinsuppdateringar och uppdateringar av inbyggd disk. hello programvara och LSI drivrutinsuppdateringar uppdateras utan avbrott och kan tillämpas via hello klassiska Azure-portalen. uppdateringar av hello disk inbyggd störande uppdateringar och kan endast användas via hello Windows PowerShell-gränssnittet för hello enhet.

Beroende på vilken version av din enhet är igång och att du kan fastställa om uppdateringen 1.2 kommer att användas. Du kan kontrollera hello programvaruversionen på enheten genom att gå toohello **snabböversikten** avsnitt i enheten **instrumentpanelen**.

</br>

| Om du kör programvaruversion... | Vad som händer i hello portal? |
| --- | --- |
| Version – GA |Om du kör versionen (GA) kan du inte installera denna uppdatering. Kontrollera [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md) tooupdate din enhet. |
| Uppdatering 0.1 |Portalen gäller Update 1.2. |
| Uppdatering 0.2 |Portalen gäller Update 1.2. |
| Uppdatering 0.3 |Portalen gäller Update 1.2. |
| Uppdatering 1 |Den här uppdateringen är inte tillgänglig. |
| Uppdatera 1.1 |Den här uppdateringen är inte tillgänglig. |

</br>

> [!IMPORTANT]
> * Du kan inte se Update 1.2 direkt, eftersom vi gör en stegvis distribution av hello uppdateringar. Söker efter uppdateringar igen några dagar som den här uppdateringen blir tillgänglig snart.
> * Uppdateringen innehåller en uppsättning inledande kontroller i manuella och automatiska toodetermine hello enhetens hälsotillstånd vad gäller maskinvara tillstånd och nätverksanslutningen. Kontrollerna före utförs endast om du använder hello uppdateringar från hello klassiska Azure-portalen.
> * Vi rekommenderar att du installerar hello programvara och drivrutinsuppdateringar via hello klassiska Azure-portalen. Du bör bara gå toohello Windows PowerShell-gränssnittet för hello enhet (tooinstall uppdateringar) om hello före uppdateringen gateway kontrollen misslyckas i hello-portalen. hello uppdateringar kan ta 5-10 timmar tooinstall (inklusive hello Windows-uppdateringar). hello Underhåll läge uppdateringar måste installeras via hello Windows PowerShell-gränssnittet för hello enhet. Eftersom Underhåll läge uppdateringar är störande uppdateringar, kommer dessa nertid för din enhet.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-hello-azure-classic-portal"></a>Installera uppdatering 1.2 via hello klassiska Azure-portalen
Utför följande steg tooupdate hello enheten för[Update 1.2](storsimple-update1-release-notes.md). Använd den här proceduren om du har en gateway som konfigurerats på DATA 0-nätverksgränssnittet på din enhet.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Kontrollera att enheten kör **StorSimple 8000 Series uppdatering 1.2 (6.3.9600.17584)**. Hej **senast uppdaterad datum** bör också ändras. Du får också se att Underhåll läge uppdateringar är tillgängliga (det här meddelandet kan fortsätta toobe som visas för dig too24 hello timmar efter installation av uppdateringar).
   
   Underhåll läge uppdateringar är störande uppdateringar som leda till enheten driftstopp och kan endast användas via hello Windows PowerShell-gränssnittet på enheten.
   
   ![Underhållssidan](./media/storsimple-install-update-1/InstallUpdate12_10M.png "sidan Underhåll")
2. Hämta hello Underhåll läge uppdateringar med hjälp av stegen i hello [toodownload snabbkorrigeringar](#to-download-hotfixes) toosearch för och hämta KB3063416 som installerar uppdateringar av inbyggd disk (hello andra uppdateringar ska vara installerad nu).
3. Följ stegen i hello [installera och verifiera Underhåll läge snabbkorrigeringar](#to-install-and-verify-maintenance-mode-hotfixes) tooinstall hello Underhåll läge uppdateringar.
4. I Hej klassiska Azure-portalen, navigera toohello **Underhåll** och längst ned hello hello-sidan, klickar du på **Sök efter uppdateringar** toocheck för alla Windows-uppdateringar och klicka sedan på **installera uppdateringar** . Du är klar när alla av hello uppdateringar har installerats.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Installera uppdatering 1.2 på en enhet som har en gateway som konfigurerats för en icke-DATA 0-nätverksgränssnittet
Du bör använda den här proceduren endast om du inte hello gateway kontroll när du försöker tooinstall hello uppdateringar via hello klassiska Azure-portalen. hello misslyckas när du har en gateway som tilldelats tooa utan DATA 0-nätverksgränssnittet och din enhet med en software version tidigare tooUpdate 1. Om enheten inte har en gateway på en icke-DATA 0-nätverksgränssnittet kan du uppdatera din enhet direkt från hello klassiska Azure-portalen. Se [uppdatering 1.2 via hello klassiska Azure-portalen](#install-update-1.2-via-the-azure-classic-portal).

hello programvaruversioner som kan uppgraderas med den här metoden är uppdatering 0.1, uppdatering 0,2 och uppdatering 0.3.

> [!IMPORTANT]
> * Om enheten kör versionen (GA) version, kontakta [Microsoft-supporten](storsimple-contact-microsoft-support.md) tooassist du med hello uppdatera.
> * Den här proceduren måste toobe utföras en gång tooapply Update 1.2. Du kan använda hello Azure klassiska portal tooapply efterföljande uppdateringar.
> 
> 

Om enheten körs före uppdateringen 1 programvara och har en gateway som angetts för ett nätverksgränssnitt än DATA 0, kan du tillämpa uppdateringen 1.2 i hello följande två sätt:

* **Alternativ 1**: hämta hello uppdateringen och använda den med hjälp av hello `Start-HcsHotfix` cmdlet från hello Windows PowerShell-gränssnittet för hello enhet. Detta är hello rekommenderad metod. **Använd inte den här metoden tooapply Update 1.2 om enheten kör uppdatering 1.0 eller uppdatera 1.1.**
* **Alternativ 2**: ta bort hello gateway-konfiguration och installation hello update direkt från hello klassiska Azure-portalen.

Detaljerade anvisningar för var och en av dessa finns i följande avsnitt hello.

## <a name="option-1-use-windows-powershell-for-storsimple-tooapply-update-12-as-a-hotfix"></a>Alternativ 1: Använd Windows PowerShell för StorSimple tooapply Update 1.2 som en snabbkorrigering
Du bör använda den här proceduren om du kör uppdatering 0.1, 0,2 0,3 och om din gateway-kontroll har misslyckats vid tooinstall uppdateringar från hello klassiska Azure-portalen. Om du kör versionen (GA) programvara du [Microsoft-supporten](storsimple-contact-microsoft-support.md) tooupdate din enhet.

tooinstall Update 1.2 som en snabbkorrigering som du måste hämta och installera följande snabbkorrigeringar hello:

| Ordning | kB | Beskrivning | Typ av uppdatering |
| --- | --- | --- | --- |
| 1 |KB3063418 |Programuppdatering |Vanliga |
| 2 |KB3043005 |LSI SAS-styrenhet uppdatering |Vanliga |
| 3 |KB3063416 |Disk inbyggd programvara |Underhåll |

Innan du använder den här proceduren tooapply hello uppdatera, se till att:

* Båda styrenheter är online.

Utför följande steg tooapply Update 1.2 hello. **hello uppdateringar kan ta cirka 2 timmar toocomplete (cirka 30 minuter för programvara, 30 minuter för drivrutin, 45 minuter för disk inbyggd programvara).**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-hello-azure-classic-portal-tooapply-update-12-after-removing-hello-gateway-configuration"></a>Alternativ 2: Använda hello Azure klassiska portal tooapply Update 1.2 efter borttagning hello gateway-konfiguration
Den här proceduren gäller endast tooStorSimple enheter som använder en programvara version tidigare tooUpdate 1 och har en gateway på ett nätverksgränssnitt än DATA 0. Du behöver tooclear hello gateway tidigare tooapplying hello uppdateringen.

hello update kan ta några timmar toocomplete. Om dina värdar finns i olika undernät, kan borttagning hello gateway-konfiguration på hello iSCSI-gränssnitt leda till driftstopp. Vi rekommenderar att du konfigurerar DATA 0 för iSCSI-trafik tooreduce hello driftstopp.

Utför följande steg toodisable hello nätverksgränssnitt med hello gateway hello och hello uppdateringen.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nästa steg
Mer information om hello [Update 1.2 versionen](storsimple-update1-release-notes.md).

