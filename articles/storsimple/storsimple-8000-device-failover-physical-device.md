---
title: aaaStorSimple redundans, disaster recovery tooa StorSimple 8000-serien fysisk enhet | Microsoft Docs
description: "Lär dig hur toofail över StorSimple 8000-serien fysisk enhet tooanother fysiska enheten."
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
ms.date: 05/03/2017
ms.author: alkohli
ms.openlocfilehash: 29d2576a96c446ff5ffcd98dcd0f5a07f1ab08ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="fail-over-tooa-storsimple-8000-series-physical-device"></a>Växla över tooa StorSimple 8000-serien fysisk enhet

## <a name="overview"></a>Översikt

Den här självstudiekursen beskriver hello steg krävs toofail via en StorSimple 8000-serien fysisk enhet tooanother fysiska StorSimple-enheten om en katastrof. StorSimple använder hello redundans funktionen toomigrate data på enheten från en fysisk enhet som källa i hello datacenter tooanother fysisk enhet. hello anvisningarna i den här kursen gäller tooStorSimple 8000-serien fysiska enheter som kör programvaruversioner uppdatering 3 och senare.

toolearn mer om enheten växling vid fel och hur den används toorecover en katastrofåterställning gå för[redundans och disaster recovery för StorSimple 8000-serien enheter](storsimple-8000-device-failover-disaster-recovery.md).

toofail över en StorSimple fysisk enhet tooa StorSimple moln-enhet, gå för[växla över tooa StorSimple moln installation](storsimple-8000-device-failover-cloud-appliance.md). toofail via en fysisk enhet tooitself gå för[växla över toohello samma fysiska StorSimple-enheten](storsimple-8000-device-failover-same-device.md).


## <a name="prerequisites"></a>Krav

- Se till att du har granskat hello överväganden för växling vid fel för enheten. Mer information finns för[vanliga överväganden för enheten redundans](storsimple-8000-device-failover-disaster-recovery.md).

- Du måste ha en StorSimple 8000-serien fysisk enhet som distribuerats i hello datacenter. hello enheten måste köra Update 3 eller senare programvaruversion. Mer information finns för[distribuera din lokala StorSimple-enhet](storsimple-8000-deployment-walkthrough-u2.md).


## <a name="steps-toofail-over-tooa-physical-device"></a>Steg toofail över tooa fysisk enhet

Utför följande steg toorestore hello din enhet tooa fysiska målenhet.

1. Kontrollera att hello volymbehållare som du vill ha toofail över har associerade molnögonblicksbilder. Mer information finns för[Använd StorSimple Enhetshanteraren service toocreate säkerhetskopieringar](storsimple-8000-manage-backup-policies-u2.md).
2. Gå tooyour StorSimple Enhetshanteraren och klicka sedan på **enheter**. I hello **enheter** bladet, gå toohello lista över enheter som är anslutna med din tjänst.
    ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev1.png)
3. Välj och klicka på din källenhet. hello källenheten har hello volymbehållare som du vill ha toofail över. Gå för**Inställningar > Volymbehållare**.
4. Välj en volymbehållare som du vill att toofail över tooanother enhet. Klicka på hello volym behållaren toodisplay hello lista över volymer i den här behållaren. Välj en volym, högerklicka och klicka på **ta Offline** tootake hello volymen offline. Upprepa proceduren för alla hello volymer i hello volymbehållare.
5. Hello Upprepa föregående steg för alla hello volymbehållare som toofail över tooanother enhet.
6. Gå tillbaka toohello **enheter** bladet. Hello kommandofältet klickar du på **växla över**.
    ![Klicka på misslyckas över](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev2.png)
    
7. I hello **växla över** bladet utföra hello följande steg:
   
   1. Klicka på **källa**. Hej volymbehållare med volymer som är kopplade till molnögonblicksbilder visas. Endast hello behållare som visas är berättigade för redundans. Välj hello volymbehållare som toofail över i hello lista över volymbehållare. **Endast hello volymbehållare med associerade molnögonblicksbilder och offline volymer visas.**

       ![Välj källa](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev5.png)
   2. Klicka på **mål**. Välj en målenhet för hello volymbehållare valde i föregående steg i hello, hello nedrullningsbara listan över tillgängliga enheter. Endast hello-enheter som har tillräcklig kapacitet tooaccommodate källa volymbehållare visas i hello.

        ![Välj mål](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev6.png)

   3. Slutligen granska alla hello växling vid fel inställningar under **sammanfattning**. När du har granskat hello inställningar, Välj hello kryssrutan som anger att hello volymer i valda volymbehållare är offline. Klicka på **OK**.

       ![Granska inställningarna för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev8.png)
  
8. StorSimple skapar en växling vid fel. Klicka på hello jobbet meddelande toomonitor hello redundans jobb via hello **jobb** bladet.

    Om hello volymbehållare som du redundansväxlade har lokala volymer, ser du enskilda återställningsjobb för varje lokal volym (inte för nivåindelade volymer) i hello behållaren. Dessa återställningspunkter jobb kan ta ganska tid toocomplete. Det är troligt att hello beställningsjobbet kan slutföra tidigare. Volymerna har lokala garantier endast när hello återställningsjobb har slutförts.

    ![Övervakningsjobb för växling vid fel](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev13.png)

9. När hello växling vid fel har slutförts går toohello **enheter** bladet.
   
   1. Välj hello-enhet som har använts som hello målenhet för hello failover-processen.

       ![Välj enhet](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev14.png)

   2. Gå toohello **Volymbehållare** bladet. Alla hello volymbehållare, tillsammans med hello volymer från hello gamla enhet, ska visas.

       ![View target volymbehållare](./media/storsimple-8000-device-failover-disaster-recovery/failover-phy-dev16.png)


## <a name="next-steps"></a>Nästa steg

* När du har utfört en växling vid fel, kanske du måste för[inaktivera eller ta bort din StorSimple-enhet](storsimple-8000-deactivate-and-delete-device.md).
* Information om hur toouse hello StorSimple Device Manager service, gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

