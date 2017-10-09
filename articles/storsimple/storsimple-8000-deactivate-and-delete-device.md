---
title: aaaDeactivate och ta bort en StorSimple 8000-serieenhet | Microsoft Docs
description: "Beskriver hur tooremove StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
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
ms.date: 05/23/2017
ms.author: alkohli
ms.openlocfilehash: 841ecd7f0fb5e425bf23e1fe0044faeab2af4b53
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-device"></a>Inaktivera och ta bort en StorSimple-enhet

## <a name="overview"></a>Översikt

Den här artikeln beskriver hur toodeactivate och ta bort en StorSimple-enhet som är anslutna tooa StorSimple enheten Manager-tjänsten. hello anvisningarna i den här artikeln gäller endast tooStorSimple 8000-serien enheter inklusive hello StorSimple moln apparater. Om du använder en virtuell StorSimple-matris går för[inaktivera och ta bort en virtuell StorSimple-matris](storsimple-virtual-array-deactivate-and-delete-device.md).

Inaktivering av servrarna hello anslutning mellan hello enhets- och hello motsvarande StorSimple Enhetshanteraren. Vill du kanske tootake StorSimple-enheten out-of-service (till exempel om du ersätter eller uppgradera din enhet eller om du inte längre använder StorSimple). I så fall måste toodeactivate hello enheten innan du kan ta bort den.

När du inaktiverar en enhet är alla data som lagras lokalt på hello enheten inte längre tillgänglig. Endast hello data som är associerade med hello-enhet som lagrats i hello molnet kan återställas.

> [!WARNING]
> Inaktivering är en PERMANENT åtgärd och kan inte ångras. En inaktiverad enhet kan inte registreras med hello StorSimple enheten Manager-tjänsten om det inte att återställa toofactory standardvärden.
>
> Hej fabriksåterställa processen tar bort alla hello-data som lagras lokalt på din enhet. Därför måste du ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet. Den här ögonblicksbild i molnet kan du toorecover alla hello data i ett senare skede.

När du har läst den här självstudiekursen kommer du att kunna:

* Inaktivera en enhet och ta bort hello data.
* Inaktivera en enhet och behålla hello data.

> [!NOTE]
> Innan du inaktiverar en fysiska StorSimple-enheten eller molnet installation, stoppa eller ta bort klienter och värdar som beror på enheten.


## <a name="deactivate-and-delete-data"></a>Inaktivera och ta bort data

Om du är intresserad av att ta bort hello enheten fullständigt och inte vill att tooretain hello data på hello enhet, slutför du hello följande steg.

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a>toodeactivate hello enheten och ta bort hello data

1. Innan du inaktiverar en enhet måste du ta bort alla hello volym behållare (och hello volymer) som är associerade med hello enhet. Du kan ta bort volymbehållare bara när du har tagit bort hello associerade säkerhetskopior.

    > [!NOTE]
    > Se till att hello data från hello bort volymbehållare tas bort från hello enheten innan du inaktiverar en fysiska StorSimple-enheten eller molnet utrustning. Du kan övervaka hello molnet förbrukning diagram och när du ser hello molnanvändning släpp på grund av hello säkerhetskopior som du har tagit bort kan du fortsätta toodeactivate hello enhet. Om du inaktiverar hello enheten innan inträffar den här nedrullningsbara hello data ensamma i hello storage-konto och påförs kostnader.

2. Inaktivera hello enhet enligt följande:
   
   1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**. I hello **enheter** bladet, Välj hello-enhet som du vill toodeactivate, högerklicka och klicka sedan på **inaktivera**.

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. I hello **inaktivera** bladet Skriv hello enheten namnet tooconfirm och klicka på **inaktivera**. hello inaktivera startar och tar några minuter toocomplete.

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)

3. Efter inaktiveringen, kan du ta bort hello enheten helt. Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello. hello-tjänsten kan sedan inte längre hantera hello bort enheten. Använd hello följande steg toodelete hello enhet:
   
   1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**. I hello **enheter** bladet, Välj hello inaktiveras enhet som du vill toodelete, högerklicka och klicka sedan på **ta bort**.

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. I hello **ta bort** bladet Skriv hello enheten namnet tooconfirm och klicka på **ta bort**. hello borttagning tar några minuter toocomplete.

        ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. När hello borttagningen är klar har du ett meddelande. hello enhetslistan uppdateras även tooreflect hello borttagning.

## <a name="deactivate-and-retain-data"></a>Inaktivera och behålla data

Om du är intresserad av att ta bort hello enhet men vill tooretain hello data Slutför hello följande steg:

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a>toodeactivate en enhet och spara hello data
1. Inaktivera hello enhet. Alla hello volymbehållare och förblir hello ögonblicksbilder av hello enhet.
   
   1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**. I hello **enheter** bladet, Välj hello-enhet som du vill toodeactivate, högerklicka och klicka sedan på **inaktivera**.

         ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate1.png)
   2. I hello **inaktivera** bladet Skriv hello enheten namnet tooconfirm och klicka på **inaktivera**. hello inaktivera startar och tar några minuter toocomplete.

         ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate2.png)
2. Nu kan du växla över hello volymbehållare och hello associerade ögonblicksbilder. Procedurer finns för[redundans och disaster recovery för din StorSimple-enhet](storsimple-8000-device-failover-disaster-recovery.md).
3. Efter inaktivering och växling vid fel, kan du ta bort hello enhet helt. Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello. hello-tjänsten kan sedan inte längre hantera hello bort enheten. toodelete hello enhet, fullständig hello följande steg:
   
   1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **enheter**. I hello **enheter** bladet, Välj hello inaktiveras enhet som du vill toodelete, högerklicka och klicka sedan på **ta bort**.

       ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate5.png)
   2. I hello **ta bort** bladet Skriv hello enheten namnet tooconfirm och klicka på **ta bort**. hello borttagning tar några minuter toocomplete.

       ![Inaktivera StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate6.png)
   3. När hello borttagningen är klar har du ett meddelande. hello enhetslistan uppdateras även tooreflect hello borttagning.

     
## <a name="deactivate-and-delete-a-cloud-appliance"></a>Inaktivera och ta bort en moln-installation

För en för StorSimple molnet, inaktivering från hello portal tar bort och tar bort hello virtuell dator och hello resurser som skapades när den etablerades. När hello molnet installation inaktiveras, kan den inte återställas tooits tidigare tillstånd.

![Inaktivera molnet StorSimple-enhet](./media/storsimple-8000-deactivate-and-delete-device/deactivate7.png)

Inaktivera resulterar i hello följande åtgärder:

* hello StorSimple moln enheten tas bort från hello-tjänsten.
* hello virtuell dator för hello StorSimple moln enheten tas bort.
* hello operativsystemdisken och datadiskar som skapats för hello StorSimple moln installation tas bort.
* hello värdbaserade tjänsten och virtuella nätverk som skapades under etableringen bevaras. Om du inte använder dessa enheter, bör du ta bort dem manuellt.
* Molnögonblicksbilder som skapats av hello StorSimple-enhet för molnet bevaras.

När hello molnet enheten har inaktiverats kan du ta bort den från hello lista över enheter. Välj hello inaktiveras enheten, högerklicka och klicka sedan på **ta bort**. StorSimple meddelar dig när hello enheten tas bort och hello listan över enheter uppdateringar.

## <a name="next-steps"></a>Nästa steg

* toorestore hello inaktiverad enhet toofactory standardinställningar finns för[återställer standardinställningarna för hello enheten toofactory](storsimple-8000-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* För teknisk support [kontaktar Microsoft Support](storsimple-8000-contact-microsoft-support.md).
* toolearn mer om hur toouse hello StorSimple Enhetshanteraren service gå för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

