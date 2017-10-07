---
title: "aaaMicrosoft Azure StorSimple virtuell matris säkerhetskopiering kursen | Microsoft Docs"
description: Beskriver hur tooback in virtuella StorSimple-matris delar och volymer.
services: storsimple
documentationcenter: NA
author: alkohli
manager: timlt
editor: 
ms.assetid: e3cdcd9e-33b1-424d-82aa-b369d934067e
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 02/27/2017
ms.author: alkohli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 7a015fd594f8f56c48fab149a2736be9dec2c24b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-shares-or-volumes-on-your-storsimple-virtual-array"></a>Säkerhetskopiera resurser eller volymer på din virtuella StorSimple-matris

## <a name="overview"></a>Översikt

hello virtuella StorSimple-matrisen är en hybrid cloud lokalt virtuell lagringsenhet som kan konfigureras som en filserver eller en iSCSI-server. hello virtuella matris kan hello användaren toocreate schemalagda och manuella säkerhetskopieringar av alla hello resurser eller volymer på hello enhet. När du har konfigurerats som en filserver, kan också objektnivååterställning. Den här självstudiekursen beskrivs hur toocreate schemalagda och manuella säkerhetskopieringar och utföra objektnivååterställning toorestore borttagna filer på din virtuella matrisen.

Den här kursen gäller toohello StorSimple virtuell matriser endast. Information om 8000-serien finns för[skapa en säkerhetskopia för enheten i 8000-serien](storsimple-manage-backup-policies-u2.md)

## <a name="back-up-shares-and-volumes"></a>Säkerhetskopiera filresurser och volymer

Säkerhetskopieringar ger tidpunkts-skydd, förbättrar återställningsmöjligheterna och minimera återställningstider för filresurser och volymer. Du kan säkerhetskopiera en resurs eller en volym på din StorSimple-enhet på två sätt: **schemalagda** eller **manuell**. Hello metoderna beskrivs i följande avsnitt hello.

## <a name="change-hello-backup-start-time"></a>Ändra hello starttiden för säkerhetskopiering

> [!NOTE]
> Schemalagda säkerhetskopieringar skapas i den här versionen av en standardprincip som körs varje dag vid en viss tidpunkt och säkerhetskopierar alla hello resurser eller volymer på hello enhet. Det är inte möjligt toocreate anpassade principer för schemalagda säkerhetskopieringar just nu.


Din virtuella StorSimple-matris har en standardprincip för säkerhetskopiering som börjar med en angiven tid på dagen (22:30) och säkerhetskopierar alla hello resurser eller volymer på hello enheten en gång om dagen. Du kan ändra hello tiden på vilka hello säkerhetskopiering startar, men hello frekvens och hello Kvarhållning (som anger hello antal säkerhetskopior tooretain) inte kan ändras. Under dessa säkerhetskopior säkerhetskopieras hello hela virtuella enheten. Detta kan potentiellt påverka hello prestanda hos hello enheten och påverkar hello arbetsbelastningar som har distribuerats på hello enhet. Därför rekommenderar vi att du schemalägger dessa säkerhetskopior för låg belastning.

 toochange hello standard säkerhetskopiering starttid, utför följande steg i hello hello [Azure-portalen](https://portal.azure.com/).

#### <a name="toochange-hello-start-time-for-hello-default-backup-policy"></a>toochange hello starttid för hello standardprincip för säkerhetskopiering

1. Gå för**enheter**. hello lista över enheter som registrerats StorSimple Device Manager-tjänsten kommer att visas. 
   
    ![Navigera toodevices](./media/storsimple-virtual-array-backup/changebuschedule1.png)

2. Välj och klicka på enheten. Hej **inställningar** bladet visas. Gå för**hantera > Säkerhetskopieringsprinciper**.
   
    ![Välj enhet](./media/storsimple-virtual-array-backup/changebuschedule2.png)

3. I hello **Säkerhetskopieringsprinciper** bladet hello standard starttiden är 22:30. Du kan ange hello Ny starttid för hello dagsschema i enheten tidszon.
   
    ![Navigera toobackup principer](./media/storsimple-virtual-array-backup/changebuschedule5.png)

4. Klicka på **Spara**.

### <a name="take-a-manual-backup"></a>Gör en manuell säkerhetskopia

Dessutom tooscheduled säkerhetskopior, du kan ta en manuell säkerhetskopiering (på begäran) av data på enheten när som helst.

#### <a name="toocreate-a-manual-backup"></a>toocreate en manuell säkerhetskopia

1. Gå för**enheter**. Välj enhet och högerklicka på **...**  på hello längst till höger i hello markerade raden. Hello snabbmenyn, Välj **ta säkerhetskopia**.
   
    ![Navigera tootake säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup1m.png)

2. I hello **ta säkerhetskopia** bladet, klickar du på **ta säkerhetskopia**. Detta ska säkerhetskopiera alla hello resurser på hello filserver eller alla hello volymer på iSCSI-servern. 
   
    ![Starta Säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup2m.png)
   
    En säkerhetskopiering på begäran startar och du ser att ett säkerhetskopieringsjobb har startats.
   
    ![Starta Säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup3m.png) 
   
    När hello jobbet har slutförts, meddelas du igen. hello säkerhetskopieringen startas.
   
    ![Säkerhetskopieringsjobbet har skapats](./media/storsimple-virtual-array-backup/takebackup4m.png)

3. tootrack hello fortskrider hello säkerhetskopieringar och titta på hello jobbinformation Klicka hello-meddelande. Du kommer för **Jobbdetaljer**.
   
     ![information om säkerhetskopieringsjobb](./media/storsimple-virtual-array-backup/takebackup5m.png)

4. När hello säkerhetskopieringen är klar, går för**Management > säkerhetskopieringskatalog**. En moln-ögonblicksbild av alla hello resurser (eller volymer) visas på enheten.
   
    ![Slutförda säkerhetskopiering](./media/storsimple-virtual-array-backup/takebackup19m.png) 

## <a name="view-existing-backups"></a>Visa befintliga säkerhetskopior
tooview hello befintliga säkerhetskopior, utföra hello följa stegen i hello Azure-portalen.

#### <a name="tooview-existing-backups"></a>tooview befintliga säkerhetskopior

1. Gå för**enheter** bladet. Välj och klicka på enheten. I hello **inställningar** bladet går för**Management > säkerhetskopieringskatalog**.
   
    ![Navigera toobackup katalog](./media/storsimple-virtual-array-backup/viewbackups1.png)
2. Ange hello följande kriterier toobe som används vid filtrering:
   
    - **Tidsintervallet** – kan vara **senaste 1 timme**, **föregående 24 timmar**, **senaste 7 dagarna**, **de senaste 30 dagarna**, **senaste året** , och **anpassade datum**.
    
    - **Enheter** – Välj hello listan över filservrar eller iSCSI-servrar har registrerats med din StorSimple Device Manager-tjänst.
   
    - **Initierade** – kan vara automatiskt **schemalagda** (genom en princip för säkerhetskopiering) eller **manuellt** initieras (av du).
   
    ![Filtrera säkerhetskopieringar](./media/storsimple-virtual-array-backup/viewbackups2.png)

3. Klicka på **Använd**. hello filtrerad lista över säkerhetskopieringar visas i hello **säkerhetskopieringskatalog** bladet. Obs endast 100 säkerhetskopiering elementen kan visas vid en given tidpunkt.
   
    ![Uppdaterade säkerhetskopieringskatalogen](./media/storsimple-virtual-array-backup/viewbackups3.png)

## <a name="next-steps"></a>Nästa steg

Lär dig mer om [administrera din virtuella StorSimple-matris](storsimple-ova-web-ui-admin.md).

