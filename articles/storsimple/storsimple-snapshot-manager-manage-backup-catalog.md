---
title: "aaaStorSimple Snapshot Manager säkerhetskopieringskatalogen | Microsoft Docs"
description: "Beskriver hur toouse hello StorSimple Snapshot Manager MMC snapin-modulen tooview och hantera hello säkerhetskopieringskatalogen."
services: storsimple
documentationcenter: NA
author: SharS
manager: timlt
editor: 
ms.assetid: 6abdbfd2-22ce-45a5-aa15-38fae4c8f4ec
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 06/05/2017
ms.author: v-sharos
ms.openlocfilehash: 173410095bcec7948d780d7fc258d2e3670bde8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-storsimple-snapshot-manager-toomanage-hello-backup-catalog"></a>Använd StorSimple Snapshot Manager toomanage hello säkerhetskopieringskatalogen

## <a name="overview"></a>Översikt
hello huvudfunktion för StorSimple Snapshot Manager är tooallow kopierar du toocreate programkonsekvent säkerhetskopiering av StorSimple-volymer i hello form av ögonblicksbilder. Ögonblicksbilder visas sedan i en XML-fil som kallas en *säkerhetskopieringskatalogen*. hello säkerhetskopieringskatalogen organiserar ögonblicksbilder av volymen grupp och sedan efter lokal ögonblicksbild eller ögonblicksbild i molnet.

Den här självstudiekursen beskriver hur du kan använda hello **säkerhetskopieringskatalog** nod toocomplete hello följande uppgifter:

* Återställa en volym
* Klona en volym eller en volym grupp
* Ta bort en säkerhetskopia
* Återställa en fil
* Återställa hello Storsimple Snapshot Manager-databasen

Du kan visa hello säkerhetskopieringskatalogen genom att expandera hello **säkerhetskopieringskatalogen** nod i hello **omfång** fönstret och expandera sedan hello volym grupp.

* Om du klickar på hello volym gruppnamn hello **resultat** visar hello antal lokala ögonblicksbilder och molnögonblicksbilder som är tillgängliga för hello volym grupp. 
* Om du klickar på **lokal ögonblicksbild** eller **ögonblicksbild i molnet**, hello **resultat** visar följande information om varje ögonblicksbild hello (beroende på din **Visa** inställningar):
  
  * **Namnet** – hello tid hello ögonblicksbilden togs.
  * **Typen** – om det här är en lokal ögonblicksbild eller en ögonblicksbild i molnet.
  * **Ägare** – hello innehållets ägare. 
  * **Tillgängliga** – om hello ögonblicksbild är tillgänglig. **True** anger den hello ögonblicksbilden är tillgängligt och kan återställas. **FALSKT** anger denna hello ögonblicksbild är inte längre tillgänglig. 
  * **Importera** – om hello säkerhetskopiering har importerats. **True** anger att hello säkerhetskopiering har importerats från hello StorSimple Enhetshanteraren tjänsten på hello tid hello enheten har konfigurerats i StorSimple Snapshot Manager; **FALSKT** anger att den inte har importerats men har skapats av StorSimple Snapshot Manager. (Det lätt att identifiera en grupp importerade volymen eftersom ett suffix läggs till som identifierar hello enheten som hello volym grupp importerades.)
    
    ![Säkerhetskopieringskatalogen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Backup_catalog.png)
* Om du expanderar **lokal ögonblicksbild** eller **ögonblicksbild i molnet**, och klicka sedan på namnet på en enskild ögonblicksbild hello **resultat** visar följande information om hello hello ögonblicksbild som du har valt:
  
  * **Namnet** – hello volymen som identifieras med en enhetsbeteckning. 
  * **Lokalt namn** – hello lokala namnet på hello-enhet (om tillgängligt). 
  * **Enheten** – hello namn på hello enheten vilka hello volymen finns. 
  * **Tillgängliga** – om hello ögonblicksbild är tillgänglig. **True** anger den hello ögonblicksbilden är tillgängligt och kan återställas. **FALSKT** anger denna hello ögonblicksbild är inte längre tillgänglig. 

## <a name="restore-a-volume"></a>Återställa en volym
Använd hello följa proceduren toorestore en volym från en säkerhetskopia.

#### <a name="prerequisites"></a>Krav
Om du inte redan har gjort det, skapa en volym och volymen gruppen och ta sedan bort hello volym. Som standard säkerhetskopierar StorSimple Snapshot Manager en volym innan du gör det möjligt att den toobe tas bort. Den här försiktighetsåtgärden kan förhindra förlust av data om hello volymen tas bort oavsiktligt eller om hello data behöver toobe återställas av någon anledning. 

StorSimple Snapshot Manager visar hello efter meddelande medan hello förebyggande säkerhetskopia skapas.

![Automatisk ögonblicksbild meddelande](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Automatic_snap.png) 

> [!IMPORTANT]
> Du kan inte ta bort en volym som är en del av en grupp för volymen. hello borttagningsalternativet är inte tillgänglig. <br>
> 
> 

#### <a name="toorestore-a-volume"></a>toorestore en volym
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager. 
2. I hello **omfång** rutan Expandera hello **säkerhetskopieringskatalog** , expandera grupperna volym och klicka sedan på **lokala ögonblicksbilder** eller **Molnögonblicksbilder**. En lista över ögonblicksbilder av säkerhetskopior visas i hello **resultat** fönstret.
3. Hitta hello säkerhetskopia som du vill toorestore, högerklicka och klicka sedan på **återställa**.
   
    ![Återställa säkerhetskopierade katalogen](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_BU_catalog.png) 
4. På sidan Bekräfta hello, granska hello information skriver **Bekräfta**, och klicka sedan på **OK**. StorSimple Snapshot Manager använder hello säkerhetskopiering toorestore hello volym.
   
    ![Återställa bekräftelsemeddelande](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Restore_volume_msg.png) 
5. Du kan övervaka hello återställning åtgärd medan den körs. I hello **omfång** rutan Expandera hello **jobb** noden och klicka sedan på **kör**. hello jobbinformation visas i hello **resultat** fönstret. När hello Återställningsjobbet har slutförts hello jobbinformation är överförda toohello **senaste 24 timmarna** lista.

## <a name="clone-a-volume-or-volume-group"></a>Klona en volym eller en volym grupp
Använd hello följa proceduren toocreate en dubblett (klona) av en volym eller en volym grupp.

#### <a name="tooclone-a-volume-or-volume-group"></a>tooclone en volym eller en volym grupp
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **säkerhetskopieringskatalog** , expandera grupperna volym och klicka sedan på **Molnögonblicksbilder**. En lista över säkerhetskopieringar visas i hello **resultat** fönstret.
3. Hitta hello volym eller volym grupp du vill tooclone, högerklicka på hello volym eller volym gruppnamn och klickar på **klona**. Hej **ögonblicksbild i molnet klona** dialogrutan visas.
   
    ![Klona en ögonblicksbild i molnet](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Fullständig hello **ögonblicksbild i molnet klona** dialogrutan enligt följande: 
   
   1. I hello **namnet** skriver du ett namn för hello klonas volym. Det här namnet visas i hello **volymer** nod. 
   2. (Valfritt) Markera **enhet**, och väljer en enhetsbeteckning hello nedrullningsbara listan.
   3. (Valfritt) Markera **mappen NTFS**, och anger en sökväg till mappen eller klicka på Bläddra och välj en plats för hello mapp. 
   4. Klicka på **Skapa**.
5. När hello Kloningen är klar måste du initiera hello klonas volym. Starta Serverhanteraren och starta sedan Diskhantering. Detaljerade instruktioner finns [montera volymer](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). När den har initierats hello-volymen visas under hello **volymer** nod i hello **omfång** fönstret. Om du inte ser hello volym i listan uppdaterar hello lista över volymer (Högerklicka på hello **volymer** noden och klicka sedan på **uppdatera**).

## <a name="delete-a-backup"></a>Ta bort en säkerhetskopia
Använd följande procedur toodelete en ögonblicksbild från hello säkerhetskopieringskatalogen hello. 

> [!NOTE]
> Tar bort en ögonblicksbild hello säkerhetskopierade data som är associerade med hello ögonblicksbild. Dock kan hello av rensa data från molnet hello ta lite tid.<br>


#### <a name="toodelete-a-backup"></a>toodelete en säkerhetskopia
1. Klicka på hello skrivbordsikon toostart StorSimple Snapshot Manager.
2. I hello **omfång** rutan Expandera hello **säkerhetskopieringskatalog** , expandera grupperna volym och klicka sedan på **lokala ögonblicksbilder** eller **Molnögonblicksbilder**. En lista över ögonblicksbilder visas i hello **resultat** fönstret.
3. Högerklicka på hello ögonblicksbild du toodelete, och klickar sedan på **ta bort**.
4. När hello-meddelande visas klickar du på **OK**.

## <a name="recover-a-file"></a>Återställa en fil
Om en fil av misstag tas bort från en volym kan du återställa hello-filen genom att hämta en ögonblicksbild som före datum hello tas bort med hjälp av hello ögonblicksbild toocreate en klon av hello volym och kopiera hello-filen från hello klonade volym toohello ursprungliga volymen.

#### <a name="prerequisites"></a>Krav
Innan du börjar måste du kontrollera att du har en aktuell säkerhetskopia av hello volym grupp. Sedan tar du bort en fil som lagras på en av hello volymerna i gruppen volym. Använd slutligen hello följande steg toorestore hello tas bort från säkerhetskopian. 

#### <a name="toorecover-a-deleted-file"></a>toorecover borttagna filer
1. Klicka på hello StorSimple Snapshot Manager ikon på skrivbordet. Hej öppnas StorSimple Snapshot Manager-konsolen. 
2. I hello **omfång** rutan Expandera hello **säkerhetskopieringskatalog** noden och bläddra tooa ögonblicksbild som innehåller hello bort filen. Normalt bör du välja en ögonblicksbild skapades innan hello borttagning.
3. Hitta hello volym som du vill tooclone, högerklicka och klicka på **klona**. Hej **ögonblicksbild i molnet klona** dialogrutan visas.
   
    ![Klona en ögonblicksbild i molnet](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_Clone.png) 
4. Fullständig hello **ögonblicksbild i molnet klona** dialogrutan enligt följande: 
   
   1. I hello **namnet** skriver du ett namn för hello klonas volym. Det här namnet visas i hello **volymer** nod. 
   2. (Valfritt) Välj **enhet**, och väljer en enhetsbeteckning hello nedrullningsbara listan. 
   3. (Valfritt) Välj **mappen NTFS**, och ange en sökväg till mappen eller klicka på **Bläddra** och välj en plats för hello-mappen. 
   4. Klicka på **Skapa**. 
5. När hello Kloningen är klar måste du initiera hello klonas volym. Starta Serverhanteraren och starta sedan Diskhantering. Detaljerade instruktioner finns [montera volymer](storsimple-snapshot-manager-manage-volumes.md#mount-volumes). När den har initierats hello-volymen visas under hello **volymer** nod i hello **omfång** fönstret. 
   
    Om du inte ser hello volym i listan uppdaterar hello lista över volymer (Högerklicka på hello **volymer** noden och klicka sedan på **uppdatera**).
6. Öppna hello NTFS-mapp som innehåller hello klonas volym, expandera hello **volymer** noden och sedan öppna hello klonas volym. Hitta hello-filen som du vill toorecover och kopiera den toohello primära volymen.
7. När du återställer hello-fil kan du ta bort hello NTFS-mapp som innehåller hello klonas volym.

## <a name="restore-hello-storsimple-snapshot-manager-database"></a>Återställa hello StorSimple Snapshot Manager-databasen
Du bör regelbundet säkerhetskopiera hello StorSimple Snapshot Manager-databasen på hello värddator. Om en olycka inträffar eller hello värddatorn misslyckas av någon anledning kan återställa du den från hello säkerhetskopia. Skapa hello databassäkerhetskopia är en manuell process.

#### <a name="tooback-up-and-restore-hello-database"></a>tooback upp och Återställ hello databasen
1. Stoppa hello Microsoft StorSimple Management-tjänsten:
   
   1. Starta Serverhanteraren.
   2. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   3. På hello **Services** fönster, Välj hello **Management-tjänsten för Microsoft StorSimple**.
   4. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **stoppa hello tjänsten**.
2. Bläddra tooC:\ProgramData\Microsoft\StorSimple\BACatalog på hello värddator. 
   
   > [!NOTE]
   > ProgramData är en dold mapp.
   > 
   > 
3. Hitta hello katalog XML-filen och kopiera hello fil lagra hello kopia på en säker plats eller i hello molnet. Du kan använda den här säkerhetskopian toohelp Återställ hello principer för säkerhetskopiering som du skapade i StorSimple Snapshot Manager om hello värd misslyckas.
   
    ![Azure StorSimple säkerhetskopiering katalogfil](./media/storsimple-snapshot-manager-manage-backup-catalog/HCS_SSM_bacatalog.png)
4. Starta om hello Microsoft StorSimple Management-tjänsten: 
   
   1. På hello instrumentpanelen Serverhanteraren på hello **verktyg** väljer du **Services**.
   2. På hello **Services** fönster, Välj hello **Management-tjänsten för Microsoft StorSimple**.
   3. I hello Högerklicka rutan under **Management-tjänsten för Microsoft StorSimple**, klickar du på **starta om tjänsten hello**.
5. Bläddra tooC:\ProgramData\Microsoft\StorSimple\BACatalog på hello värddator. 
6. Ta bort hello katalog XML-filen och Ersätt den med hello säkerhetskopian som du skapade. 
7. Klicka på hello skrivbord StorSimple Snapshot Manager ikonen toostart StorSimple Snapshot Manager. 

## <a name="next-steps"></a>Nästa steg
* Lär dig mer om [med hjälp av StorSimple Snapshot Manager tooadminister StorSimple-lösningen](storsimple-snapshot-manager-admin.md).
* Lär dig mer om [StorSimple Snapshot Manager aktiviteter och arbetsflöden](storsimple-snapshot-manager-admin.md#storsimple-snapshot-manager-tasks-and-workflows).

