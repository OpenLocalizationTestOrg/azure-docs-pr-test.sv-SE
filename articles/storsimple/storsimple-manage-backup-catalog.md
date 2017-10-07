---
title: "aaaManage StorSimple säkerhetskopiering katalogen | Microsoft Docs"
description: "Förklarar hur toouse hello StorSimple Manager-tjänsten säkerhetskopieringskatalogen sidan toolist markerar och tar bort säkerhetskopior för en volym."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: ad81bee9-fe43-40b3-a384-b15fb274ecd9
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 04/28/2016
ms.author: v-sharos
ms.openlocfilehash: 14f565c174a10da2c9e2f934a533a5e493f77226
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-toomanage-your-backup-catalog"></a>Använd hello StorSimple Manager-tjänsten toomanage katalogen för säkerhetskopieringar
## <a name="overview"></a>Översikt
Hej StorSimple Manager-tjänsten **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas vid manuell eller schemalagda säkerhetskopieringar. Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.

Den här självstudiekursen beskrivs hur toolist markerar och ta bort en säkerhetskopia. toolearn hur toorestore enheten från en säkerhetskopia, gå för[återställa enheten från en säkerhetskopia](storsimple-restore-from-backup-set.md). hur tooclone en volym, gå för toolearn[klona en StorSimple-volym](storsimple-clone-volume.md).

![Säkerhetskopieringskatalogen](./media/storsimple-manage-backup-catalog/backupcatalog.png) 

Hej **säkerhetskopieringskatalogen** innehåller en fråga toonarrow valet av säkerhetskopian. Du kan filtrera hello säkerhetskopior som hämtas, baserat på hello följande parametrar:

* **Enheten** – hello enhet på vilken hello säkerhetskopian skapades.
* **Säkerhetskopiera princip- eller** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.
* **Från och till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.

hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:

* **Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.
* **Storlek** – hello verkliga storleken hos hello säkerhetskopia.
* **Skapad den** – hello datum och tid då hello säkerhetskopior skapades. 
* **Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder. En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enhet, medan en ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i molnet hello. Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.
* **Initierades av** – hello säkerhetskopieringar kan initieras automatiskt av ett schema eller manuellt av en användare. Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior. Du kan också använda hello **ta säkerhetskopia** alternativet tootake en manuell säkerhetskopiering.

## <a name="list-backup-sets-for-a-volume"></a>Lista över säkerhetskopior för en volym
Slutför följande steg toolist hello alla hello säkerhetskopior för en volym.

#### <a name="toolist-backup-sets"></a>toolist säkerhetskopior
1. På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.
2. Filtrera hello val på följande sätt:
   
   1. Välj lämplig hello-enhet.
   2. Välj en volym tooview hello motsvarande hello säkerhetskopieringar i hello nedrullningsbara listan.
   3. Ange hello tidsintervall.
   4. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute den här frågan.
      
      hello säkerhetskopior som är associerade med hello valda volymen ska visas i hello lista över säkerhetskopior.

## <a name="select-a-backup-set"></a>Välj en säkerhetskopia
Slutför hello följande steg tooselect en säkerhetskopia av en princip för volym eller säkerhetskopiering.

#### <a name="tooselect-a-backup-set"></a>tooselect en säkerhetskopia
1. På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.
2. Filtrera hello val på följande sätt:
   
   1. Välj lämplig hello-enhet.
   2. Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.
   3. Ange hello tidsintervall.
   4. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute den här frågan.
      
      hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.
3. Markera och utöka en säkerhetskopia. Hej **återställa** och **ta bort** alternativ visas på hello hello sidans nederkant. Du kan utföra någon av dessa åtgärder på hello säkerhetskopia som du har valt.

## <a name="delete-a-backup-set"></a>Ta bort en säkerhetskopia
Ta bort en säkerhetskopiering när du inte längre vill tooretain hello data som är associerade med den. Utföra hello följande steg toodelete en säkerhetskopia.

#### <a name="toodelete-a-backup-set"></a>toodelete en säkerhetskopia
1. På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog fliken**.
2. Filtrera hello val på följande sätt:
   
   1. Välj lämplig hello-enhet.
   2. Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.
   3. Ange hello tidsintervall.
   4. Klicka på kryssikonen hello ![Kryssikon](./media/storsimple-manage-backup-catalog/HCS_CheckIcon.png) tooexecute den här frågan.
      
      hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.
3. Markera och utöka en säkerhetskopia. Hej **återställa** och **ta bort** alternativ visas på hello hello sidans nederkant. Klicka på **Ta bort**.
4. Du meddelas när hello borttagning pågår och när den har slutförts. Uppdatera hello frågan på den här sidan när hello borttagningen är klar. hello bort säkerhetskopia visas inte längre i hello lista över säkerhetskopior.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[Använd hello säkerhetskopieringskatalogen toorestore enheten från en säkerhetskopia](storsimple-restore-from-backup-set.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

