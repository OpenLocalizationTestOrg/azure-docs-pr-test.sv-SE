---
title: "aaaChange lösenord via StorSimple Enhetshanteraren | Microsoft Docs"
description: "Beskriver hur hello toouse StorSimple Manager service toochange administratörslösenord StorSimple Snapshot Manager och enhet."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: f178509c-f4e1-48a8-90b2-d4ad050eeb30
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: b2836eb4d3a05e1d2a5eeeeefe66c75f63ba38ed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toochange-your-storsimple-passwords"></a>Använd hello StorSimple Manager-tjänsten toochange StorSimple-lösenord
## <a name="overview"></a>Översikt
Hej klassiska Azure-portalen **konfigurera** innehåller alla hello enheten parametrar som du kan konfigurera om på en StorSimple-enhet som hanteras av en StorSimple Manager-tjänsten. Den här självstudiekursen beskrivs hur du kan använda hello **konfigurera** sidan toochange enhetsadministratören eller lösenordet för StorSimple Snapshot Manager.

## <a name="change-hello-device-administrator-password"></a>Ändra hello enhetens administratörslösenord
När du använder Windows PowerShell-gränssnittet tooaccess hello StorSimple-enhet är obligatoriska tooenter administratörslösenord för enheten. När hello första StorSimple-enhet är registrerad med en tjänst hello standardlösenordet för det här gränssnittet är *Password1*. Hello säkerheten för dina data, du är obligatoriska toochange lösenordet vid hello slutet av hello registreringsprocessen. Du kan avsluta från hello registreringen utan att ändra lösenordet. Mer information finns i [steg3: konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

hello lösenordet som angavs först via Windows PowerShell-gränssnittet för hello under registreringen kan sedan ändras via hello klassiska Azure-portalen. Utför följande steg toochange hello enhetens administratörslösenord hello.

#### <a name="toochange-hello-device-administrator-password"></a>toochange hello enhetens administratörslösenord
1. I klassiska hello-portalen klickar du på **enheter** > **konfigurera** för din enhet.
2. Bläddra nedåt toohello **administratörslösenordet för enheten** avsnitt. Ange ett administratörslösenord som innehåller från 8 too15 tecken. hello lösenordet måste vara en kombination av 3 eller flera av versaler, gemener, siffror och särskilda tecken.
3. Bekräfta hello lösenord.
4. Klicka på **spara** på hello hello sidans nederkant.

hello enhetens administratörslösenord ska nu uppdateras. Du kan använda den här ändrade lösenord tooaccess hello Windows PowerShell-gränssnittet.

## <a name="change-hello-storsimple-snapshot-manager-password"></a>Ändra hello lösenordet för StorSimple Snapshot Manager
Programvaran StorSimple Manager finns på din Windows-värd och kan administratörer toomanage säkerhetskopior av StorSimple-enheten i hello form av lokala och molnögonblicksbilder.

När du konfigurerar en enhet i StorSimple Snapshot Manager, uppmanas du att tooprovide hello enhetens IP-adress och lösenord tooauthenticate lagringsenheten. Det här lösenordet först konfigureras via hello Windows PowerShell-gränssnittet. Mer information finns i [steg3: konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](storsimple-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

hello lösenordet som angavs först via Windows PowerShell-gränssnittet för hello under registreringen kan sedan ändras via hello klassiska portalen. Utför följande steg toochange hello StorSimple Snapshot Manager lösenord hello.

#### <a name="toochange-hello-storsimple-snapshot-manager-password"></a>lösenordet för toochange hello StorSimple Snapshot Manager
1. I klassiska hello-portalen klickar du på **enheter** > **konfigurera** för din enhet.
2. Bläddra nedåt toohello **StorSimple Snapshot Manager** avsnitt. Ange ett lösenord som är 14 eller 15 tecken. Kontrollera att lösenordet hello innehåller en kombination av 3 eller flera av versaler, gemener, siffror och särskilda tecken.
3. Bekräfta hello lösenord.
4. Klicka på **spara** på hello hello sidans nederkant.

hello lösenordet för StorSimple Snapshot Manager ska nu uppdateras.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [StorSimple-säkerhet](storsimple-security.md).
* Lär dig mer om [ändra din enhetskonfiguration](storsimple-modify-device-config.md).
* Lär dig mer om [med hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

