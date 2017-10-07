---
title: "aaaChange StorSimple-lösenord | Microsoft Docs"
description: "Beskriver hur hello toouse StorSimple Enhetshanteraren service toochange administratörslösenord StorSimple Snapshot Manager och enhet."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: cf884be31b4bbf9e372c0aa11b9da2eadcda35dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toochange-your-storsimple-passwords"></a>Använd hello StorSimple Enhetshanteraren service toochange StorSimple-lösenord

## <a name="overview"></a>Översikt
hello Azure-portalen **Enhetsinställningar** alternativet innehåller alla hello enheten parametrar som du kan konfigurera om på en StorSimple-enhet som hanteras av en tjänst för StorSimple Enhetshanteraren. Den här självstudiekursen beskrivs hur du kan använda hello **säkerhet** alternativ **Enhetsinställningar** toochange enhetsadministratören eller lösenordet för StorSimple Snapshot Manager.

## <a name="change-hello-device-administrator-password"></a>Ändra hello enhetens administratörslösenord
När du använder Windows PowerShell-gränssnittet tooaccess hello StorSimple-enhet är obligatoriska tooenter administratörslösenord för enheten. När hello första StorSimple-enhet är registrerad med en tjänst hello standardlösenordet för det här gränssnittet är *Password1*. Hello säkerheten för dina data, du är obligatoriska toochange lösenordet vid hello slutet av hello registreringsprocessen. Du kan avsluta från hello registreringen utan att ändra lösenordet. Mer information finns i [steg3: konfigurera och registrera hello enheten via Windows PowerShell för StorSimple](storsimple-8000-deployment-walkthrough-u2.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

hello-lösenord som först har angetts via Windows PowerShell-gränssnittet för hello under registreringen kan ändras senare via hello Azure-portalen. Utför följande steg toochange hello enhetens administratörslösenord hello.

#### <a name="toochange-hello-device-administrator-password"></a>toochange hello enhetens administratörslösenord
1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.

2. Välj hello tabular lista över enheter, och klicka på hello enhet vars lösenord du avser toochange.

    ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. I hello **inställningar** bladet går för**Enhetsinställningar > säkerhet**.

    ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. I hello **säkerhetsinställningar** bladet, klickar du på **lösenord** toochange hello enhetens administratörslösenord.

    ![](./media/storsimple-8000-change-passwords/changepwd3.png)

5. I hello **lösenord** bladet, ange ett administratörslösenord som innehåller från 8 too15 tecken. hello lösenordet måste vara en kombination av 3 eller flera av versaler, gemener, siffror och särskilda tecken.

6. Bekräfta hello lösenord.

    ![](./media/storsimple-8000-change-passwords/changepwd4.png)

7. Klicka på **spara** och när du uppmanas att bekräfta på **Ja**.

    ![](./media/storsimple-8000-change-passwords/changepwd6.png)

hello enhetens administratörslösenord ska nu uppdateras. Du kan använda den här ändrade lösenord tooaccess hello Windows PowerShell-gränssnittet.

## <a name="set-hello-storsimple-snapshot-manager-password"></a>Ange hello lösenordet för StorSimple Snapshot Manager
Programvaran StorSimple Manager finns på din Windows-värd och kan administratörer toomanage säkerhetskopior av StorSimple-enheten i hello form av lokala och molnögonblicksbilder.

När du konfigurerar en enhet i StorSimple Snapshot Manager, uppmanas du att tooprovide hello enhetens IP-adress och lösenord tooauthenticate lagringsenheten.

Du kan ange eller ändra hello lösenordet för StorSimple Snapshot Manager via hello Azure-portalen. Utför följande steg tooset hello eller ändra hello lösenordet för StorSimple Snapshot Manager.

#### <a name="tooset-hello-storsimple-snapshot-manager-password"></a>lösenordet för tooset hello StorSimple Snapshot Manager
1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**.

2. Välj hello tabular lista över enheter, och klicka på hello enhet vars lösenordet för StorSimple Snapshot Manager du avser tooset eller ändra.

     ![](./media/storsimple-8000-change-passwords/changepwd1.png)

3. I hello **inställningar** bladet går för**Enhetsinställningar > säkerhet**.

     ![](./media/storsimple-8000-change-passwords/changepwd2.png)

4. I hello **säkerhetsinställningar** bladet, klickar du på **lösenord** lösenordet för StorSimple Snapshot Manager hello tooset eller ändra.

     ![](./media/storsimple-8000-change-passwords/changepwd3.png) 

5. I hello **lösenord** bladet, ange ett lösenord som är 14 eller 15 tecken. Kontrollera att lösenordet hello innehåller en kombination av 3 eller flera av versaler, gemener, siffror och särskilda tecken.

6. Bekräfta hello lösenord.

     ![](./media/storsimple-8000-change-passwords/changepwd5.png)

7. Klicka på **spara** och när du uppmanas att bekräfta på **Ja**.

     ![](./media/storsimple-8000-change-passwords/changepwd6.png)

hello lösenordet för StorSimple Snapshot Manager ska nu uppdateras.

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [StorSimple-säkerhet](storsimple-8000-security.md).
* Lär dig mer om [ändra din enhetskonfiguration](storsimple-8000-modify-device-config.md).
* Lär dig mer om [med hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

