---
title: aaaUpdate din StorSimple-enhet | Microsoft Docs
description: "Förklarar hur toouse hello StorSimple uppdatera funktionen tooinstall regelbundet och underhåll läge uppdateringar och snabbkorrigeringar."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 786059f5-2a38-4105-941d-0860ce4ac515
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/18/2016
ms.author: v-sharos
ms.openlocfilehash: 05acf05c8fc89bbb4343f67ad103235bbe3dba0a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="update-your-storsimple-8000-series-device"></a>Uppdatera enheten StorSimple 8000-serien
## <a name="overview"></a>Översikt
Hej StorSimple uppdateringar funktionerna kan du tooeasily hålla din StorSimple-enhet som är uppdaterade. Du kan tillämpa uppdateringar toohello enheten via hello klassiska Azure-portalen eller via Windows PowerShell-gränssnittet för hello beroende på hello uppdateringstyp. Den här självstudiekursen beskriver hello uppdateringstyperna och hur tooinstall av dem.

Du kan använda två typer av uppdateringar: 

* Vanliga (eller normalläge) uppdateringar
* Underhåll läge uppdateringar

Du kan installera regelbundna uppdateringar via hello klassiska Azure-portalen eller Windows PowerShell; dock måste du använda Windows PowerShell tooinstall Underhåll läge uppdateringar. 

Varje uppdatering är separat, nedan.

### <a name="regular-updates"></a>Regelbundna uppdateringar
Regelbundna uppdateringar är icke-störande uppdateringar som kan installeras när hello enheten är i normalläge. Uppdateringarna tillämpas via hello Microsoft Update-webbplatsen tooeach enhetskontroll. 

> [!IMPORTANT]
> En domänkontrollant och växling vid fel kan uppstå under hello uppdateringsprocessen. Men påverkar detta inte systemets tillgänglighet eller åtgärden.
> 
> 

* Mer information om hur tooinstall regelbundet uppdaterar via hello klassiska Azure-portalen finns [installera regelbundna uppdateringar via hello klassiska Azure-portalen](#install-regular-updates-via-the-azure-classic-portal).
* Du kan också installera regelbundna uppdateringar via Windows PowerShell för StorSimple. Mer information finns i [installerar regelbundna uppdateringar via Windows PowerShell för StorSimple](#install-regular-updates-via-windows-powershell-for-storsimple).

### <a name="maintenance-mode-updates"></a>Underhåll läge uppdateringar
Underhåll läge uppdateringar är störande uppdateringar, till exempel disk firmware-uppgraderingar. De här uppdateringarna kräver hello enheten toobe försätta i underhållsläge. Mer information finns i [steg 2: Ange underhållsläge](#step2). Du kan inte använda hello Azure klassiska portal tooinstall Underhåll läge uppdateringar. I stället måste du använda Windows PowerShell för StorSimple. 

Mer information om hur tooinstall underhållsläge uppdaterar finns [installera Underhåll läge uppdateringar via Windows PowerShell för StorSimple](#install-maintenance-mode-updates-via-windows-powershell-for-storsimple).

> [!IMPORTANT]
> Underhållsläge uppdateringar måste vara tillämpas separat tooeach domänkontrollant. 
> 
> 

## <a name="install-regular-updates-via-hello-azure-classic-portal"></a>Installera regelbundna uppdateringar via hello klassiska Azure-portalen
Du kan använda hello Azure klassiska portal tooapply uppdateringar tooyour StorSimple-enhet.

[!INCLUDE [storsimple-install-updates-manually](../../includes/storsimple-install-updates-manually.md)]

## <a name="install-regular-updates-via-windows-powershell-for-storsimple"></a>Installera regelbundna uppdateringar via Windows PowerShell för StorSimple
Alternativt kan du använda Windows PowerShell för StorSimple tooapply regular (normalt läge) uppdateringar.

> [!IMPORTANT]
> Du kan installera regelbundna uppdateringar med hjälp av Windows PowerShell för StorSimple, rekommenderar vi att du installerar regelbundna uppdateringar via hello klassiska Azure-portalen. Från och med uppdatering 1, kommer inledande kontroller att utföras tidigare tooinstalling uppdateringar från hello-portalen. Kontrollerna före åsidosätter fel och ge en jämnare upplevelse. 
> 
> 

[!INCLUDE [storsimple-install-regular-updates-powershell](../../includes/storsimple-install-regular-updates-powershell.md)]

## <a name="install-maintenance-mode-updates-via-windows-powershell-for-storsimple"></a>Installera Underhåll läge uppdateringar via Windows PowerShell för StorSimple
Du kan använda Windows PowerShell för StorSimple tooapply Underhåll läge uppdateringar tooyour StorSimple-enhet. Alla i/o-begäranden är pausade i det här läget. Tjänster, till exempel beständiga RAM-minne (NVRAM) eller hello klustertjänsten också har stoppats. Både domänkontrollanter startas om när du anger eller avsluta det här läget. När du lämnar det här läget återupptas alla hello-tjänster och bör vara felfritt. (Detta kan ta några minuter.)

Om du behöver tooapply Underhåll läge uppdateringar visas en avisering via hello klassiska Azure-portalen som du har uppdateringar som måste installeras. Den här aviseringen innehåller instruktioner för att använda Windows PowerShell för StorSimple tooinstall hello uppdateringar. När du har uppdaterat enheten använda hello samma procedur toochange hello enhet tooRegular-läge. Stegvisa instruktioner finns [steg 4: avsluta underhållsläget](#step4).

> [!IMPORTANT]
> * Innan du anger underhållsläge, kontrollera att båda styrenheter är felfri genom att kontrollera hello **maskinvarustatus** på hello **Underhåll** sida i hello klassiska Azure-portalen. Kontakta Microsofts Support för hello nästa steg om hello domänkontrollanten inte är felfri. Mer information finns i tooContact Microsoft-supporten. 
> * När du är i underhållsläge, måste tooapply hello uppdatering först på en domänkontrollant och sedan på hello andra domänkontrollanter.
> 
> 

### <a name="step-1-connect-toohello-serial-console-a-namestep1"></a>Steg 1: Anslut toohello seriekonsol<a name="step1">
Använd först ett program som PuTTY tooaccess hello seriekonsol. hello nedan förklaras hur toouse PuTTY tooconnect toohello seriekonsol.

[!INCLUDE [storsimple-use-putty](../../includes/storsimple-use-putty.md)]

### <a name="step-2-enter-maintenance-mode-a-namestep2"></a>Steg 2: Ange underhållsläge<a name="step2">
När du har anslutit toohello konsolen avgöra om det finns uppdateringar tooinstall och ange Underhåll läge tooinstall dem.

[!INCLUDE [storsimple-enter-maintenance-mode](../../includes/storsimple-enter-maintenance-mode.md)]

### <a name="step-3-install-your-updates-a-namestep3"></a>Steg 3: Installera uppdateringar<a name="step3">
Installera uppdateringarna.

[!INCLUDE [storsimple-install-maintenance-mode-updates](../../includes/storsimple-install-maintenance-mode-updates.md)]

### <a name="step-4-exit-maintenance-mode-a-namestep4"></a>Steg 4: Avsluta underhållsläge<a name="step4">
Slutligen ska du avsluta underhållsläget.

[!INCLUDE [storsimple-exit-maintenance-mode](../../includes/storsimple-exit-maintenance-mode.md)]

## <a name="install-hotfixes-via-windows-powershell-for-storsimple"></a>Installera snabbkorrigeringar via Windows PowerShell för StorSimple
Till skillnad från uppdateringar för Microsoft Azure StorSimple installeras snabbkorrigeringarna från en delad mapp. Precis som med uppdateringar, finns det två typer av snabbkorrigeringar: 

* Vanliga snabbkorrigeringar 
* Underhåll läge snabbkorrigeringar  

hello följande procedurer beskriver hur toouse Windows PowerShell för StorSimple tooinstall regelbundet och underhåll läge snabbkorrigeringar.

[!INCLUDE [storsimple-install-regular-hotfixes](../../includes/storsimple-install-regular-hotfixes.md)]

[!INCLUDE [storsimple-install-maintenance-mode-hotfixes](../../includes/storsimple-install-maintenance-mode-hotfixes.md)]

## <a name="what-happens-tooupdates-if-you-perform-a-factory-reset-of-hello-device"></a>Vad händer tooupdates om du utför en återställning av hello enheten?
Om en enhet är Återställ toofactory inställningar kan förloras alla hello-uppdateringar. När hello fabriksåterställning enheten är registrerad och konfigurerad, måste toomanually installera uppdateringar via hello klassiska Azure-portalen och/eller Windows PowerShell för StorSimple. Läs mer om fabriksåterställning [återställer standardinställningarna för hello enheten toofactory](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [använder Windows PowerShell för StorSimple tooadminister StorSimple-enheten](storsimple-windows-powershell-administration.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

