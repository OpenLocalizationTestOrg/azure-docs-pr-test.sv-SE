---
title: "principer för säkerhetskopiering aaaStorSimple Snapshot Manager | Microsoft Docs"
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen toocreate och hantera hello säkerhetskopiering principer som styr schemalagda säkerhetskopieringar."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: c2ae75a8d0568090add6018da18de73eb56e6590
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-backup-policies"></a>Använda StorSimple Snapshot Manager toocreate och hantera principer för säkerhetskopiering
## <a name="overview"></a>Översikt
En princip för säkerhetskopiering skapar ett schema för att säkerhetskopiera volymdata lokalt eller i hello molnet. När du skapar en princip för säkerhetskopiering måste ange du också en bevarandeprincip. (Du kan lagra maximalt 64 ögonblicksbilder.) Mer information om principer för säkerhetskopiering finns [säkerhetskopiera typer](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies) i [StorSimple 8000-serien: en hybridmolnlösningen](storsimple-overview.md).

Den här självstudiekursen beskrivs hur du:

* Skapa en princip för säkerhetskopiering
* Redigera en princip för säkerhetskopiering
* Ta bort en princip för säkerhetskopiering

## <a name="create-a-backup-policy"></a>Skapa en princip för säkerhetskopiering
Använd följande procedur toocreate en ny säkerhetskopieringsprincip hello.

#### <a name="toocreate-a-backup-policy"></a>toocreate en princip för säkerhetskopiering
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** fönstret högerklickar du på **Säkerhetskopieringsprinciper**, och klicka på **skapa princip för säkerhetskopiering**.

    ![Skapa en princip för säkerhetskopiering](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Hej **skapa en princip** dialogrutan visas.

    ![Skapa en princip – fliken Allmänt](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. På hello **allmänna** fliken, fullständig hello följande information:

   1. I hello **namnet** skriver du ett namn för hello principen.
   2. I hello **volym grupp** textruta, hello-typnamn för hello volym gruppen som är associerad med hello principen.
   3. Välj antingen **lokal ögonblicksbild** eller **molnet ögonblicksbild**.
   4. Välj hello antalet ögonblicksbilder tooretain. Om du väljer **alla**, 64 ögonblicksbilder ska behållas (hello maximala).
4. Klicka på hello **schema** fliken.

    ![Skapa en princip - fliken schema](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. På hello **schema** fliken, fullständig hello följande information:

   1. Klicka på hello **aktivera** kryssrutan tooschedule hello nästa säkerhetskopiering.
   2. Under **inställningar**väljer **en gång**, **dagliga**, **veckovisa**, eller **månatliga**.
   3. I hello **starta** textrutan, klicka hello kalendern och välj ett startdatum.
   4. Under **avancerade inställningar**, kan du ange valfria Upprepa scheman och ett slutdatum.
   5. Klicka på **OK**.

När du har skapat en princip för säkerhetskopiering hello följande information visas i hello **resultat** fönstret:

* **Namnet** – hello namn på princip för säkerhetskopiering.
* **Typen** – lokal ögonblicksbild eller ögonblicksbild i molnet.
* **Volymen grupp** – hello volym gruppen som är associerad med hello principen.
* **Kvarhållning** – hello antalet ögonblicksbilder behålls; hello högsta 64.
* **Skapa** – hello datum som principen skapades.
* **Aktiverad** – om hello principen är för närvarande i praktiken: **True** anger att det är aktivt; **FALSKT** anger att det inte är aktiv.

## <a name="edit-a-backup-policy"></a>Redigera en princip för säkerhetskopiering
Använd följande procedur tooedit en säkerhetskopieringsprincip hello.

#### <a name="tooedit-a-backup-policy"></a>tooedit en princip för säkerhetskopiering
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på hello **Säkerhetskopieringsprinciper** nod. Alla principer för säkerhetskopiering av hello visas i hello **resultat** fönstret.
3. Högerklicka på hello princip du vill tooedit och klicka sedan på **redigera**.

    ![Redigera en princip för säkerhetskopiering](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. När hello **skapa en princip** visas, gör ändringarna och klicka sedan på **OK**.

## <a name="delete-a-backup-policy"></a>Ta bort en princip för säkerhetskopiering
Använd hello följa proceduren toodelete en princip för säkerhetskopiering.

#### <a name="toodelete-a-backup-policy"></a>toodelete en princip för säkerhetskopiering
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan klickar du på hello **Säkerhetskopieringsprinciper** nod. Alla principer för säkerhetskopiering av hello visas i hello **resultat** fönstret.
3. Högerklicka på hello säkerhetskopieringsprincip som du vill toodelete och klicka sedan på **ta bort**.
4. När hello-meddelande visas klickar du på **Ja**.

    ![Princip för säkerhetskopiering bekräftelse på borttagning](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).
* Lär dig hur för[använda StorSimple Snapshot Manager tooview och hantera säkerhetskopieringsjobb](storsimple-snapshot-manager-manage-backup-jobs.md).
