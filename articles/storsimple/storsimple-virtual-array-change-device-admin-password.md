---
title: "administratörslösenord för aaaChange virtuella StorSimple-matris enhet | Microsoft Docs"
description: "Beskriver hur toouse antingen hello Azure-portalen eller virtuella StorSimple-matris web UI toochange hello enhetens administratörslösenord."
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: 11490814-d9fd-4dc7-9c3b-55dd2c23eaf1
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 531b395df7aeade0a909360797c6b0f0abd9fd1e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="change-hello-storsimple-virtual-array-device-administrator-password-via-storsimple-device-manager"></a>Ändra hello virtuella StorSimple-matris enhetens administratörslösenord via StorSimple Enhetshanteraren

## <a name="overview"></a>Översikt

När du använder hello Windows PowerShell-gränssnittet tooaccess hello virtuella StorSimple-matrisen är du obligatoriska tooenter administratörslösenord för enheten. När hello StorSimple-enheten först etablerats och igång, hello standardlösenordet är *Password1*. För hello säkerheten för dina data, hello standardlösenordet upphör att gälla hello första gången du loggar in och du är obligatoriska toochange lösenordet.

Du kan också använda antingen hello lokala web användargränssnitt eller hello Azure portal toochange hello administratörslösenord för enheten när som helst efter hello enheten har distribuerats i produktionsmiljön. Var och en av de här procedurerna beskrivs i den här artikeln.

 ![Bladet för enheter](./media/storsimple-virtual-array-change-device-admin-password/ova-devices-blade.png)

## <a name="use-hello-azure-portal-toochange-hello-password"></a>Använd hello Azure portal toochange hello lösenord

Utföra hello följande steg toochange hello enhetens administratörslösenord via hello Azure-portalen.

#### <a name="toochange-hello-device-administrator-password-via-hello-azure-portal"></a>Hej toochange enhetens administratörslösenord via hello Azure-portalen

1. Markera din tjänst på hello Landningssida för tjänsten, genom att dubbelklicka på hello tjänstnamn och sedan i hello **Management** klickar du på **enheter**. Då öppnas hello **enheter** blad som visar en lista över alla virtuella StorSimple-matris-enheter.

2. I hello **enheter** bladet dubbelklickar på hello-enhet som kräver en ändring av lösenord.

3. I hello **inställningar** bladet för din enhet klickar du på **säkerhet**.

4. I hello **säkerhetsinställningar** bladet hello följande:
   
   1. Bläddra nedåt toohello **administratörslösenordet för enheten** avsnitt. Ange ett administratörslösenord som innehåller från 8 too15 tecken.
   2. Bekräfta hello lösenord.
   3. Klicka på **spara** hello överst i hello-bladet.

hello enhetens administratörslösenord har uppdaterats. Du kan använda den här ändrade lösenord tooaccess hello enheten lokalt.

![Säkerhet inställningsbladet](./media/storsimple-virtual-array-change-device-admin-password/ova-change-device-pwd.png)

## <a name="use-hello-local-web-ui-toochange-hello-password"></a>Använd hello lokala web UI toochange hello lösenord

Utför följande steg toochange hello enhetens administratörslösenord via hello lokala webbgränssnittet hello.

#### <a name="toochange-hello-device-administrator-password-via-hello-local-web-ui"></a>Hej toochange enhetens administratörslösenord via hello lokala webbgränssnittet

1. I hello lokala webbgränssnittet, klickar du på **Underhåll** > **lösenordsändring** för din enhet.
   
    ![Ändra password1](./media/storsimple-virtual-array-change-device-admin-password/image40.png)
2. Ange hello **nuvarande lösenord**.
3. Ange en **nytt lösenord**. hello lösenordet måste vara minst 8 tecken. Det måste innehålla 3 av 4 av hello följande: versaler, gemener, siffror och särskilda tecken.
   
    Observera att lösenordet inte får vara hello samma som hello senast 24 lösenord.
4. Ange hello lösenord tooconfirm den.
   
    ![Ändra Lösenord2](./media/storsimple-virtual-array-change-device-admin-password/image41.png)
5. Hello längst hello-sidan, klickar du på **tillämpa**. hello nytt lösenord används nu. Om hello lösenordsändring inte lyckas kan du se hello följande fel:
   
    ![felaktigt lösenord](./media/storsimple-virtual-array-change-device-admin-password/image42.png)
   
    När hello lösenord har uppdaterats, visas ett meddelande. Du kan sedan använda den här ändrade lösenord tooaccess hello enheten lokalt.


## <a name="next-steps"></a>Nästa steg
Lär dig hur för[administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

