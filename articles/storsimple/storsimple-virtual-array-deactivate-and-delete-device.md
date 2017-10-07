---
title: "aaaDeactivate och ta bort en virtuell matris för Microsoft Azure StorSimple | Microsoft Docs"
description: "Beskriver hur tooremove StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: a929f5bc-03e2-4b01-b925-973db236f19f
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2016
ms.author: alkohli
ms.openlocfilehash: b1f3ddb5822d19965739777e238af19b507df984
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-virtual-array"></a>Inaktivera och ta bort en virtuell StorSimple-matris

## <a name="overview"></a>Översikt

När du inaktiverar en virtuell StorSimple-matris bryta hello anslutning mellan hello enhets- och hello motsvarande StorSimple Enhetshanteraren. Den här självstudiekursen beskrivs hur du:

* Inaktivera en enhet 
* Ta bort en inaktiverad enhet

hello informationen i den här artikeln gäller tooStorSimple virtuella matriser. Information i 8000-serien finns toohow för[inaktivera eller ta bort en enhet](storsimple-deactivate-and-delete-device.md).

## <a name="when-toodeactivate"></a>När toodeactivate?

Inaktivering är en PERMANENT åtgärd och kan inte ångras. Du kan inte registrera en inaktiverad enhet med hello StorSimple Enhetshanteraren tjänsten igen. Du kanske behöver toodeactivate och ta bort en virtuell StorSimple-matris på hello följande scenarier:

* **Planerad redundans** : enheten är online och du planerar toofail över din enhet. Om du planerar tooupgrade tooa större enhet, eventuellt toofail över din enhet. När hello data äganderätten överförs och hello redundansväxlingen är klar, tas hello källenheten bort automatiskt.
* **Oplanerad växling** : enheten är offline och du behöver toofail över hello enhet. Det här scenariot kan uppstå under en katastrofåterställning när det finns ett avbrott i hello datacenter och den primära enheten är inte tillgänglig. Du kan planera toofail över hello enheten tooa sekundär enhet. När hello data äganderätten överförs och hello redundansväxlingen är klar, tas hello källenheten bort automatiskt.
* **Inaktivera** : du vill toodecommission hello enhet. Detta kräver toofirst inaktivera hello enhet och ta bort den. När du inaktiverar en enhet kan du inte längre komma åt alla data som lagras lokalt. Du kan endast åtkomst och Återställ hello data som lagras i hello molnet. Om du planerar tookeep hello enhetsdata efter inaktiveringen, bör du ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet. Den här ögonblicksbild i molnet kan du toorecover alla hello data i ett senare skede.

## <a name="deactivate-a-device"></a>Inaktivera en enhet

toodeactivate enheten, utföra hello följande steg.

#### <a name="toodeactivate-hello-device"></a>toodeactivate hello enhet

1. Gå för i din tjänst**Management > enheter**. I hello **enheter** bladet, klickar du på och väljer hello-enhet som du vill toodeactivate.
   
    ![Välj enhet toodeactivate](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete7.png)
2. I din **enheten instrumentpanelen** bladet, klickar du på **... Flera** och välj hello listan **inaktivera**.
   
    ![Klicka på Inaktivera](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete8.png)
3. I hello **inaktivera** bladet, typen hello enhetsnamnet och klicka sedan på **inaktivera**. 
   
    ![Bekräfta inaktivera](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete1.png)
   
    hello inaktivera startar och tar några minuter toocomplete.
   
    ![Inaktivera pågår](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete2.png)
4. Efter inaktiveringen uppdaterar hello lista över enheter.
   
    ![Inaktivera klar](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete3.png)
   
    Nu kan du ta bort den här enheten.

## <a name="delete-hello-device"></a>Ta bort hello-enhet

En enhet har toobe första inaktiverade toodelete den. Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello. hello-tjänsten kan sedan inte längre hantera hello bort enheten. hello data som är associerade med hello enhet men finns kvar i hello molnet. Dessa data sedan påförs kostnader.

toodelete hello enhet, utför följande steg hello.

#### <a name="toodelete-hello-device"></a>toodelete hello enhet

1. I din StorSimple Enhetshanteraren, gå för**Management > enheter**. I hello **enheter** bladet Välj en inaktiverad enhet som du vill toodelete.
2. I hello **enheten instrumentpanelen** bladet, klickar du på **... Flera** och klicka sedan på **ta bort**.
   
   ![Välj enhet toodelete](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete4.png)
3. I hello **ta bort** bladet, ange hello namnet på din enhet tooconfirm hello borttagning och klicka sedan på **ta bort**. Ta bort hello enheten tar inte bort hello molndata som är associerade med hello enhet. 
   
   ![Bekräfta borttagning](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete5.png) 
4. hello borttagning startar och tar några minuter toocomplete.
   
   ![Ta bort pågår](./media/storsimple-virtual-array-deactivate-and-delete-device/deactivate-delete6.png)
   
    När hello enheten har tagits bort kan se du hello uppdatera listan över enheter.

## <a name="next-steps"></a>Nästa steg

* Mer information om hur toofail över, gå för[redundans och återställning vid återställning av din virtuella StorSimple-matris](storsimple-virtual-array-failover-dr.md).

* toolearn mer om hur toouse hello StorSimple Enhetshanteraren service gå för[Använd hello StorSimple Enhetshanteraren service tooadminister din virtuella StorSimple-matris](storsimple-virtual-array-manager-service-administration.md). 

