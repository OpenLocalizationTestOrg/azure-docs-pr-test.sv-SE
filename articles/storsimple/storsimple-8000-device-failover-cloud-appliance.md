---
title: aaaStorSimple redundans, disaster recovery tooa StorSimple moln installation | Microsoft Docs
description: "Lär dig hur toofail över din StorSimple 8000-serien fysisk enhet tooa cloud installation."
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
ms.date: 07/03/2017
ms.author: alkohli
ms.openlocfilehash: e8a0bca057024358e3a557fe85a42ddefea36cff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooyour-storsimple-cloud-appliance"></a>Växla över tooyour StorSimple-enhet för molnet

## <a name="overview"></a>Översikt

Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple 8000-serien fysisk enhet tooa StorSimple-enhet för molnet om en katastrof. StorSimple använder hello redundans funktionen toomigrate data på enheten från en fysisk enhet som källa i hello datacenter tooa moln installation körs i Azure. hello riktlinjerna i den här kursen gäller tooStorSimple 8000-serien fysiska enheter och molnet installationer med programvaruversioner uppdatering 3 och senare.

toolearn mer om enheten växling vid fel och hur den används toorecover en katastrofåterställning gå för[redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).

toofail via en fysisk enhet tooanother fysiska StorSimple-enheten, gå för[växla över fysiska tooa StorSimple-enheten](storsimple-8000-device-failover-physical-device.md). toofail via en enhet tooitself gå för[växla över toohello samma fysiska StorSimple-enheten](storsimple-8000-device-failover-same-device.md).

## <a name="prerequisites"></a>Krav

- Se till att du har granskat hello överväganden för växling vid fel för enheten. Mer information finns för[vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).

- Du måste ha en StorSimple-molnet enhet skapas och konfigureras innan du kör den här proceduren. Om körs uppdaterar 3 programvaruversionen eller senare, Överväg att använda en 8020 moln anordning för hello DR. modellen för hello 8020 har 64 TB och Premium-lagring. Mer information finns för[distribuera och hantera en StorSimple-enhet för molnet](storsimple-8000-cloud-appliance-u2.md).

## <a name="steps-toofail-over-tooa-cloud-appliance"></a>Steg toofail över tooa moln-enhet

Utföra hello följande steg toorestore hello enheten tooa mål StorSimple-enhet för molnet.

1.  Kontrollera att hello volymbehållare som du vill ha toofail över har associerade molnögonblicksbilder. Mer information finns för[Använd StorSimple Enhetshanteraren service toocreate säkerhetskopieringar](storsimple-8000-manage-backup-policies-u2.md).
2. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**. I hello **enheter** bladet, gå toohello lista över enheter som är anslutna med din tjänst.
    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev1.png)
3. Välj och klicka på din källenhet. hello källenheten har hello volymbehållare som du vill ha toofail över. Gå för**Inställningar > Volymbehållare**.

    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev2.png)
    
4. Välj en volymbehållare som du vill att toofail över tooanother enhet. Klicka på hello volym behållaren toodisplay hello lista över volymer i den här behållaren. Välj en volym, högerklicka och klicka på **ta Offline** tootake hello volymen offline.

    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev5.png)

5. Upprepa proceduren för alla hello volymer i hello volymbehållare.

     ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev7.png)

6. Hello Upprepa föregående steg för alla hello volymbehållare som toofail över tooanother enhet.

7. Gå tillbaka toohello **enheter** bladet. Hello kommandofältet klickar du på **växla över**.

    ![Klicka på misslyckas över](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev8.png)
8. I hello **växla över** bladet utföra hello följande steg:
   
    1. Klicka på **källa**. Välj hello volym behållare toofail över. **Endast hello volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**
        ![Välj källa](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev11.png)
    2. Klicka på **mål**. Välj ett mål moln installation från hello nedrullningsbara listan över tillgängliga enheter. **Endast hello-enheter som har tillräcklig kapacitet tooaccommodate källa volymbehållare visas i hello.**

        ![Välj mål](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev12.png)

    3. Granska inställningarna för hello växling vid fel under **sammanfattning** och välj hello kryssrutan som anger att hello volymer i valda volymbehållare är offline. 

        ![Granska inställningarna för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-cloud-dev13.png)

9. Det skapas ett jobb för växling vid fel. toomonitor hello redundans jobbet, klicka på meddelandet om hello.

    ![Övervakningsjobb för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

10. När hello växling vid fel är klar går du tillbaka toohello **enheter** bladet.

    1. Välj hello-enhet som används som mål för hello hello växling vid fel.

       ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

    2. Klicka på **Volymbehållare**. Alla hello volymbehållare, tillsammans med hello volymer från hello gamla enhet, ska visas.

       Om hello volymbehållare som du redundansväxlade har lokalt Fäst volymer, har volymerna redundansväxlats som nivåindelade volymer. Lokalt fästa volymer stöds inte på en StorSimple-enhet för molnet.

       ![View target volymbehållare](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev17.png)


## <a name="next-steps"></a>Nästa steg

* När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).

* Information om hur toouse hello StorSimple Device Manager service, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

