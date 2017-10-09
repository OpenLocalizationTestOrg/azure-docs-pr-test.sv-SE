---
title: "aaaRestore en StorSimple-volym från en säkerhetskopia | Microsoft Docs"
description: "Förklarar hur hello toouse StorSimple Manager service säkerhetskopieringskatalogen sidan toorestore en StorSimple-volym från en säkerhetskopia."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b979782e-3184-4465-ad5f-e4516a5885d2
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e0efa74b14603be41af0cfc5400de3c39ab8824e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-storsimple-volume-from-a-backup-set"></a>Återställa en StorSimple-volym från en säkerhetskopia
[!INCLUDE [storsimple-version-selector-restore-from-backup](../../includes/storsimple-version-selector-restore-from-backup.md)]

## <a name="overview"></a>Översikt
Hej **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs. Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.

 ![Säkerhetskopiera katalog](./media/storsimple-restore-from-backup-set/HCS_BackupCatalog.png)

Den här självstudiekursen beskrivs hur toouse hello **säkerhetskopieringskatalogen** sidan toorestore en volym på din enhet från en säkerhetskopia.

## <a name="how-toouse-hello-backup-catalog"></a>Hur toouse hello säkerhetskopieringskatalogen
Hej **säkerhetskopieringskatalogen** innehåller en fråga som hjälper dig att toonarrow valet av säkerhetskopian. Du kan filtrera hello säkerhetskopior som hämtas baserat på hello följande parametrar:

* **Enheten** – hello enhet på vilken hello säkerhetskopian skapades.
* **Säkerhetskopiera princip** eller **volym** – hello säkerhetskopieringsprincip eller volym som är associerade med den här säkerhetskopian.
* **Från** och **till** – hello intervallet för datum och tid när hello säkerhetskopian skapades.

hello filtrerade säkerhetskopior visas sedan som en tabell baserad på hello följande attribut:

* **Namnet** – hello namn på hello säkerhetskopieringsprincip eller volym som är associerade med hello säkerhetskopia.
* **Storlek** – hello verkliga storleken hos hello säkerhetskopia.
* **Skapas på** – hello datum och tid då hello säkerhetskopior skapades. 
* **Typen** – säkerhetskopior kan lokala ögonblicksbilder eller molnbaserade ögonblicksbilder. En lokal ögonblicksbild är en säkerhetskopia av din volymdata som lagras lokalt på hello enhet, medan en ögonblicksbild i molnet refererar toohello säkerhetskopiering av volymdata som finns i molnet hello. Lokala ögonblicksbilder ger snabbare åtkomst medan molnögonblicksbilder väljs för dataåterhämtning.
* **Initierad av** – hello säkerhetskopieringar kan initieras automatiskt enligt tooa schema eller manuellt av en användare. (Du kan använda en princip för säkerhetskopiering tooschedule säkerhetskopior. Du kan också använda hello **ta säkerhetskopia** alternativet tootake en interaktiv säkerhetskopiering.)

## <a name="how-toorestore-your-storsimple-volume-from-a-backup"></a>Hur toorestore StorSimple-volym från en säkerhetskopia
Du kan använda hello **säkerhetskopieringskatalogen** sidan toorestore StorSimple-volym från en specifik säkerhetskopia. 

> [!WARNING]
> Återställa från en säkerhetskopia ersätter hello befintliga volymer från hello säkerhetskopia. Detta kan orsaka hello förlust av alla data som har skrivits när hello säkerhetskopian skapades.
> 
> 

Se till att hello volym är offline innan du startar en återställning på en volym. Du behöver tootake hello volym offline på värden för hello först och sedan hello enhet. Gör så hello i [kopplar från en volym](storsimple-manage-volumes.md#take-a-volume-offline). Utför följande steg toorestore en volym från en säkerhetskopia hello.

### <a name="toorestore-from-a-backup-set"></a>toorestore från en säkerhetskopia
1. På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken.
   
    ![Säkerhetskopieringskatalogen](./media/storsimple-restore-from-backup-set/HCS_Restore.png)
2. Välj en säkerhetskopia på följande sätt:
   
   1. Välj lämplig hello-enhet.
   2. Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.
   3. Ange hello tidsintervall.
   4. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png) tooexecute den här frågan.
      
      hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.
3. Expandera hello säkerhetskopia tooview hello associerade volymer. Dessa volymer måste vara offline på hello värd och enheten innan du kan återställa dem. Gör så hello i [kopplar från en volym](storsimple-manage-volumes.md#take-a-volume-offline).
   
   > [!IMPORTANT]
   > Kontrollera att du har vidtagit hello volymer offline på värden för hello först innan du utför hello volymer på hello enhet. Om du inte vidtar hello volymer offline på värden för hello leda potentiellt toodata skadas.
   > 
   > 
4. Välj en säkerhetskopia. Klicka på **återställa** på hello hello sidans nederkant.
5. Du uppmanas att bekräfta. 
   
    ![Bekräftelsesida](./media/storsimple-restore-from-backup-set/HCS_ConfirmRestore.png)
6. Granska hello återställning information och klicka på kryssikonen hello ![kryssikon](./media/storsimple-restore-from-backup-set/HCS_CheckIcon.png). Detta kommer att initiera en återställningsjobbet som du kan visa genom att öppna hello **jobb** sidan. 
7. När hello återställningen är slutförd, kan du kontrollera att hello innehållet i volymerna som ersätts av volymer från hello säkerhetskopia.

![Video tillgänglig](./media/storsimple-restore-from-backup-set/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur du kan använda hello klona och återställa funktioner i StorSimple toorecover borttagna filer, klickar du på [här](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[hantera StorSimple-volymer](storsimple-manage-volumes.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

