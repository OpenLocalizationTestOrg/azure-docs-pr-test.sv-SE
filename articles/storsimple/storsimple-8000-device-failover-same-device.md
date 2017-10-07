---
title: "aaaStorSimple redundans, katastrofåterställning för enheter i 8000-serien | Microsoft Docs"
description: "Lär dig hur toofail över din StorSimple-enhet toohello samma enhet."
services: storsimple
documentationcenter: 
author: alkohli
manager: timlt
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/23/2017
ms.author: alkohli
ms.openlocfilehash: b0b4216c7af6745ff68b85ca3d655691b43b4334
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-your-storsimple-physical-device-toosame-device"></a>Växla över StorSimple-enheten fysisk enhet toosame

## <a name="overview"></a>Översikt

Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple 8000-serien fysisk enhet tooitself om en katastrof. StorSimple använder hello redundans funktionen toomigrate data på enheten från en fysisk enhet som källa i hello datacenter tooanother fysisk enhet. hello anvisningarna i den här kursen gäller tooStorSimple 8000-serien fysiska enheter som kör programvaruversioner uppdatering 3 och senare.

toolearn mer om enheten växling vid fel och hur den används toorecover en katastrofåterställning gå för[redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).

toofail via en fysisk enhet tooanother fysisk enhet, gå för[växla över toohello samma fysiska StorSimple-enheten](storsimple-8000-device-failover-physical-device.md). toofail över en StorSimple fysisk enhet tooa StorSimple moln-enhet, gå för[växla över tooa StorSimple moln installation](storsimple-8000-device-failover-cloud-appliance.md).


## <a name="prerequisites"></a>Krav

- Se till att du har granskat hello överväganden för växling vid fel för enheten. Mer information finns för[vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).


## <a name="steps-toofail-over-toohello-same-device"></a>Steg toofail över toohello samma enhet

Utför följande steg om du behöver toofail över toohello hello samma enhet.

1. Skapa molnögonblicksbilder av alla hello volymer i enheten. Mer information finns för[Använd StorSimple Enhetshanteraren service toocreate säkerhetskopieringar](storsimple-8000-manage-backup-policies-u2.md).
2. Återställa din enhet toofactory standardvärden. Följ hello detaljerade instruktioner i [hur tooreset toofactory en StorSimple-enheten standardinställningar](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
3. Gå toohello StorSimple enheten Manager-tjänsten och välj sedan **enheter**. I hello **enheter** bladet hello gamla enheten ska visa **Offline**.

    ![Källenheten offline](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev2.png)

4. Konfigurera din enhet och registrera den igen med din StorSimple Device Manager-tjänst. hello nyregistrerade enheten ska visa **klar tooset in**. hello enhetsnamnet för hello ny enhet är hello samma som hello gamla enheten men läggas till med numeriska tooindicate hello enheten var Återställ toofactory standard och registreras igen.

    ![Nyligen registrerade enheten redo tooset in](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev3.png)
5. Slutför hello Enhetsinställningar för nya hello-enhet. Mer information finns för[steg 4: Slutför minimala Enhetsinstallationen](storsimple-8000-deployment-walkthrough-u2.md#step-4-complete-minimum-device-setup). På hello **enheter** bladet hello hello enhet ändras status för**Online**.

   > [!IMPORTANT]
   > **Slutför hello minimikonfiguration först eller din DR kan misslyckas.**

    ![Nyligen registrerade enhet online](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev7.png)

6. Välj hello gamla enhet (offline status) och från hello kommandofältet på **växla över**. I hello **växla över** bladet välj gamla enhet som hello källa och ange hello målenhet som hello nyligen registrerad enhet.

    ![Sammanfattning för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev11.png)

    Detaljerade anvisningar finns för[växla över tooanother fysisk enhet](#fail-over-to-another-physical-device).

7. En enhet Återställningsjobbet har skapats att du kan övervaka från hello **jobb** bladet.

8. När hello jobbet har slutförts, komma åt hello ny enhet och gå toohello **volymbehållare** bladet. Kontrollera att alla hello volymbehållarna från hello gamla enheten har migrerats toohello ny enhet.

   ![Volymbehållare migreras](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev13.png)

9. När hello redundansväxlingen är klar kan du inaktivera och ta bort gamla hello-enhet från hello-portalen. Välj hello gamla (offline), högerklicka på enheten och välj sedan **inaktivera**. Hello status för hello enheten uppdateras när hello enheten inaktiveras.

     ![Källenheten inaktiveras](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev14.png)

10. Välj hello inaktiveras enheten, högerklicka och välj sedan **ta bort**. Detta tar bort hello enheten från hello lista över enheter.

    ![Källenheten har tagits bort](./media/storsimple-8000-device-failover-disaster-recovery/failover-single-dev15.png)



## <a name="next-steps"></a>Nästa steg

* När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).
* Information om hur toouse hello StorSimple Device Manager service, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

