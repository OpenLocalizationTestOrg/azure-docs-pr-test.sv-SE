---
title: "aaaManage StorSimple säkerhetskopiering katalogen | Microsoft Docs"
description: "Förklarar hur toouse hello StorSimple Enhetshanteraren service säkerhetskopieringskatalogen sidan toolist markerar och tar bort säkerhetskopior."
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
ms.date: 06/29/2017
ms.author: alkohli
ms.openlocfilehash: e464609e74409a06a198790719abd82ed03c03d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-toomanage-your-backup-catalog"></a>Använd hello StorSimple Enhetshanteraren service toomanage katalogen för säkerhetskopieringar
## <a name="overview"></a>Översikt
Hej StorSimple Enhetshanteraren service **säkerhetskopieringskatalogen** bladet visar alla hello säkerhetskopior som skapas vid manuell eller schemalagda säkerhetskopieringar. Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.

Den här självstudiekursen beskrivs hur toolist markerar och ta bort en säkerhetskopia. toolearn hur toorestore enheten från en säkerhetskopia, gå för[återställa enheten från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md). hur tooclone en volym, gå för toolearn[klona en StorSimple-volym](storsimple-8000-clone-volume-u2.md).

![Säkerhetskopieringskatalogen](./media/storsimple-8000-manage-backup-catalog/bucatalog.png) 

Hej **säkerhetskopieringskatalogen** bladet innehåller en fråga toonarrow valet av säkerhetskopian. Du kan filtrera hello säkerhetskopior som hämtas, baserat på hello följande parametrar:

* **Enheten** – hello enhet på vilken hello säkerhetskopian skapades.
* **Princip för säkerhetskopiering eller volym** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.
* **Från och till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.

hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:

* **Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.
* **Storlek** – hello verkliga storleken hos hello säkerhetskopia.
* **Skapad den** – hello datum och tid då hello säkerhetskopior skapades. 
* **Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder. En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enhet, medan en ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i molnet hello. Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.
* **Initierades av** – hello säkerhetskopieringar kan initieras automatiskt av ett schema eller manuellt av en användare. Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior. Du kan också använda hello **ta säkerhetskopia** alternativet tootake en manuell säkerhetskopiering.

## <a name="list-backup-sets-for-a-backup-policy"></a>Lista över säkerhetskopior för en princip för säkerhetskopiering
Slutför följande steg toolist hello alla hello säkerhetskopieringar för en princip för säkerhetskopiering.

#### <a name="toolist-backup-sets"></a>toolist säkerhetskopior
1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **säkerhetskopieringskatalog**.

2. Filtrera hello val på följande sätt:
   
   1. Ange hello tidsintervall.
   2. Välj lämplig hello-enhet.
   3. Filtrera efter **säkerhetskopiera princip** tooview hello motsvarande hello säkerhetskopieringar.
   3. Hello säkerhetskopieringsprincip listrutan, Välj **alla** tooview alla hello säkerhetskopieringar på hello valda enheten.
   4. Klicka på **tillämpa** tooexecute den här frågan.
      
      hello säkerhetskopior som är associerade med hello valt säkerhetskopieringsprincip ska visas i hello lista över säkerhetskopior.

      ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

## <a name="select-a-backup-set"></a>Välj en säkerhetskopia
Slutför hello följande steg tooselect en säkerhetskopia av en princip för volym eller säkerhetskopiering.

#### <a name="tooselect-a-backup-set"></a>tooselect en säkerhetskopia
1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **säkerhetskopieringskatalog**.
2. Filtrera hello val på följande sätt:
   
   1. Ange hello tidsintervall. 
   2. Välj lämplig hello-enhet. 
   3. Filtrera efter volym eller säkerhetskopiering princip för hello säkerhetskopiering tooselect gärna.
   4. Klicka på **tillämpa** tooexecute den här frågan.
      
      hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.

      ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Markera och utöka en säkerhetskopia. Du kan nu se hello säkerhetskopior uppdelat efter hello volymer som den innehåller. Hej **återställa** och **ta bort** alternativ är tillgängliga via hello snabbmeny (högerklicka) för hello säkerhetskopian. Du kan utföra någon av dessa åtgärder på hello säkerhetskopia som du har valt.

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog2.png)

## <a name="delete-a-backup-set"></a>Ta bort en säkerhetskopia
Ta bort en säkerhetskopiering när du inte längre vill tooretain hello data som är associerade med den. Utföra hello följande steg toodelete en säkerhetskopia.

#### <a name="toodelete-a-backup-set"></a>toodelete en säkerhetskopia
 Gå tooyour StorSimple enheten Manager-tjänsten och klicka på **säkerhetskopieringskatalog**.
2. Filtrera hello val på följande sätt:
   
   1. Ange hello tidsintervall. 
   2. Välj lämplig hello-enhet. 
   3. Filtrera efter volym eller säkerhetskopiering princip för hello säkerhetskopiering tooselect gärna.
   4. Klicka på **tillämpa** tooexecute den här frågan.
      
      hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.

      ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog1.png)

3. Markera och utöka en säkerhetskopia. Du kan nu se hello säkerhetskopior uppdelat efter hello volymer som den innehåller. Hej **återställa** och **ta bort** alternativ är tillgängliga via hello snabbmeny (högerklicka) för hello säkerhetskopian. Högerklicka på hello valda säkerhetskopian och hello snabbmenyn, Välj **ta bort**.

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog3.png)

4. När du uppmanas att bekräfta granskar hello visas information och klickar på **ta bort**. hello valda säkerhetskopian tas bort permanent.

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog4.png)  

5. Du meddelas när hello borttagning pågår och när den har slutförts. Uppdatera hello frågan på den här sidan när hello borttagningen är klar. hello bort säkerhetskopia visas inte längre i hello lista över säkerhetskopior.

    ![Gå toobackup katalog](./media/storsimple-8000-manage-backup-catalog/bucatalog7.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[Använd hello säkerhetskopieringskatalogen toorestore enheten från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).
* Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

