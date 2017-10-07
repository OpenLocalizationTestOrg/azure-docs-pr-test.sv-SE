---
title: aaaStorSimple Snapshot Manager volym grupper | Microsoft Docs
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen toocreate och hantera grupper för volymen."
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 7a232414-6a28-4b81-bd7b-cf61e28b33d7
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: f7830b62761db7aa5139456b555b6168fb6a85de
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toocreate-and-manage-volume-groups"></a>Använda StorSimple Snapshot Manager toocreate och hantera grupper av volym
## <a name="overview"></a>Översikt
Du kan använda hello **volym grupper** nod på hello **omfång** tooassign volymer toovolume grupper, visa information om en volym-grupp, schemalägga säkerhetskopieringar och redigera volymen grupper.

Volymen grupper är pooler för relaterade volymer används tooensure att säkerhetskopiorna är programkonsekventa. Mer information finns i [volymer och volymen grupper](storsimple-what-is-snapshot-manager.md#volumes-and-volume-groups) och [integrering med Windows Volume Shadow Copy-tjänsten](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).

> [!IMPORTANT]
> * Alla volymer i en volym-grupp måste komma från en enda molntjänstleverantören.
> * När du konfigurerar volymen grupper Blanda inte klusterdelade volymer (CSV) och icke-CSV: er i hello samma grupp av volymen. StorSimple Snapshot Manager stöder inte en blandning av CSV: er och icke-CSV: er i hello samma ögonblicksbild.

![Volymen Gruppnod](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Volume_groups.png)

**Bild 1: StorSimple Snapshot Manager volym Gruppnod** 

Den här självstudiekursen beskriver hur du kan använda StorSimple Snapshot Manager till:

* Visa information om volym-grupper
* Skapa en volym-grupp
* Säkerhetskopiera en volym-grupp
* Redigera en grupp av volym
* Ta bort en volym-grupp

Alla dessa åtgärder är också tillgängliga på hello **åtgärder** fönstret.

## <a name="view-volume-groups"></a>Visa grupper av volym
Om du klickar på hello **volym grupper** nod, hello **resultat** visar hello följande information om varje volym-grupp, beroende på hello kolumnmarkeringarna du gör. (hello kolumner i hello **resultat** rutan kan konfigureras. Högerklicka på hello **volymer** väljer **visa**, och välj sedan **Lägg till/ta bort kolumner**.)

| Resultaten kolumn | Beskrivning |
|:--- |:--- |
| Namn |Hej **namn** kolumnen innehåller hello hello volym gruppens namn. |
| Program |Hej **program** kolumnen visar hello antal VSS-skrivare som är installerade och körs på hello Windows-värd. |
| Vald |Hej **valda** kolumnen visar hello antalet volymer som ingår i gruppen för hello-volym. Noll (0) anger att inget program som är associerad med hello volymer i hello volym grupp. |
| Importera |Hej **importerade** kolumnen visar hello antalet importerade volymer. När värdet för**SANT**, denna kolumn indikerar att en volym-grupp har importerats från hello Azure-portalen och inte har skapats i StorSimple Snapshot Manager. |

> [!NOTE]
> StorSimple Snapshot Manager volym grupper visas också på hello **Säkerhetskopieringsprinciper** fliken i hello Azure-portalen.
> 
> 

## <a name="create-a-volume-group"></a>Skapa en volym-grupp
Använd hello följa proceduren toocreate en volym-grupp.

#### <a name="toocreate-a-volume-group"></a>toocreate en volym-grupp
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** fönstret högerklickar du på **volym grupper**, och klicka sedan på **skapa volymen grupp**.
   
    ![Skapa volymen grupp](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Create_volume_group.png)
   
    Hej **skapa en volym grupp** dialogrutan visas.
   
    ![Skapa en volym grupp dialog](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_CreateVolumeGroup_dialog.png)
3. Ange hello följande information:
   
   1. I hello **namn** Skriv ett unikt namn för hello ny volym grupp.
   2. I hello **program** rutan, Välj program som är associerade med hello volymer som du lägger till toohello volym grupp.
      
       Hej **program** Listar de program som använder StorSimple-volymer och VSS-skrivare har aktiverats för dem. En VSS-skrivare aktiveras bara om alla hello volymer att hello-skrivaren är medveten om är StorSimple-volymer. Om hello program rutan är tom, installeras inga program som använder Azure StorSimple-volymer och har stöd för VSS-skrivare. (För närvarande Azure StorSimple stöder Microsoft Exchange och SQL Server.) Mer information om VSS-skrivare finns [integrering med Windows Volume Shadow Copy-tjänsten](storsimple-what-is-snapshot-manager.md#integration-with-windows-volume-shadow-copy-service).
      
       Om du väljer ett program, väljs automatiskt alla volymer som är kopplade till den. Å andra sidan om du väljer volymer som är associerade med ett specifikt program hello programmet väljs automatiskt i hello **program** rutan. 
   3. I hello **volymer** väljer StorSimple-volymer tooadd toohello volym gruppen. 
      
      * Du kan inkludera volymer med en eller flera partitioner. (Flera volymer för partitionen kan vara dynamiska diskar eller standarddiskar med flera partitioner.) En volym som innehåller flera partitioner behandlas som en enda enhet. Därför, om du lägger till endast en av hello partitioner tooa volym gruppen Alla hello andra partitioner är automatiskt tillagda toothat volym gruppera vid hello samma tid. När du har lagt till en flera partition volym tooa volym grupp hello flera partition volym fortsätter toobe behandlas som en enda enhet.
      * Du kan skapa grupper med tomma volym genom att inte tilldela alla volymer toothem. 
      * Blanda inte klusterdelade volymer (CSV) och icke-CSV: er i hello samma grupp av volymen. StorSimple Snapshot Manager stöder inte en blandning av CSV-volymer och icke-CSV-volymer i hello samma ögonblicksbild.
4. Klicka på **OK** toosave hello volym grupp.

## <a name="back-up-a-volume-group"></a>Säkerhetskopiera en volym-grupp
Använd hello följa proceduren tooback upp en volym-grupp.

#### <a name="tooback-up-a-volume-group"></a>tooback upp en volym-grupp
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **volym grupper** , högerklickar du på namnet på en volym grupp och klicka sedan på **ta säkerhetskopia**.
   
    ![Säkerhetskopiera volym grupp omedelbart](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_Take_backup.png)
3. I hello **ta säkerhetskopia** dialogrutan **lokal ögonblicksbild** eller **ögonblicksbild i molnet**, och klicka sedan på **skapa**.
   
    ![Ta säkerhetskopiering](./media/storsimple-snapshot-manager-manage-volume-groups/HCS_SSM_TakeBackup_dialog.png)
4. tooconfirm hello säkerhetskopiering körs, expandera hello **jobb** noden och klicka sedan på **kör**. hello säkerhetskopiering ska visas.
5. tooview hello slutförts ögonblicksbild, expandera hello **säkerhetskopieringskatalog** nod, expanderar hello volym gruppnamnet och klickar sedan **lokal ögonblicksbild** eller **ögonblicksbild i molnet**. hello säkerhetskopiering visas om den har slutförts.

## <a name="edit-a-volume-group"></a>Redigera en grupp av volym
Använd hello följa proceduren tooedit en volym-grupp.

#### <a name="tooedit-a-volume-group"></a>tooedit en volym-grupp
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **volym grupper** , högerklickar du på namnet på en volym grupp och klicka sedan på **redigera**.
3. hello ** skapa en volym grupp ** dialogrutan visas. Du kan ändra hello **namn**, **program**, och **volymer** poster.
4. Klicka på **OK** toosave ändringarna.

## <a name="delete-a-volume-group"></a>Ta bort en volym-grupp
Använd hello följa proceduren toodelete en volym-grupp. 

> [!WARNING]
> Det här tas även bort alla hello säkerhetskopior som är associerat med hello volym grupp.
> 
> 

#### <a name="toodelete-a-volume-group"></a>toodelete en volym-grupp
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **volym grupper** , högerklickar du på namnet på en volym grupp och klicka sedan på **ta bort**.
3. Hej **ta bort volymen grupp** dialogrutan visas. Typen **Bekräfta** i hello textrutan och klicka sedan på **OK**.
   
    hello bort volymen grupp försvinner hello listan i hello **resultat** rutan och alla säkerhetskopior som är associerade med gruppen volym tas bort från hello säkerhetskopieringskatalogen.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[med StorSimple Snapshot Manager tooadminister din StorSimple-lösning](storsimple-snapshot-manager-admin.md).
* Lär dig hur för[använda StorSimple Snapshot Manager toocreate och hantera principer för säkerhetskopiering](storsimple-snapshot-manager-manage-backup-policies.md).

