---
title: aaaClone StorSimple-volym | Microsoft Docs
description: "Beskriver hello klona av olika typer och när toouse dem, och förklarar hur du kan använda en säkerhetskopia tooclone en enskild volym."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b5d615f2-02a7-4222-9867-6c0385ce748c
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/17/2016
ms.author: alkohli
ms.openlocfilehash: e98d28db1abeb515ba78ab5860e7c5eba0dfcbb5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-manager-service-tooclone-a-volume"></a>Använd hello StorSimple Manager-tjänsten tooclone en volym
[!INCLUDE [storsimple-version-selector-clone-volume](../../includes/storsimple-version-selector-clone-volume.md)]

## <a name="overview"></a>Översikt
Hej StorSimple Manager-tjänsten **säkerhetskopieringskatalogen** sidan visas alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs. Du kan använda den här sidan toolist alla hello säkerhetskopior för en princip för säkerhetskopiering eller en volym, Välj eller ta bort eller använda en säkerhetskopiering toorestore eller klona en volym.

![Säkerhetskopieringskatalogen sida](./media/storsimple-clone-volume/HCS_BackupCatalog.png)  

Den här självstudiekursen beskrivs hur du kan använda en säkerhetskopia tooclone en enskild volym. Här förklaras också hello skillnaden mellan *tillfälligt* och *permanent* klonar. 

## <a name="create-a-clone-of-a-volume"></a>Skapa en klon av en volym
Du kan skapa en klon av hello samma enhet, en annan enhet eller även en virtuell dator med hjälp av en lokal eller en ögonblicksbild i molnet.

#### <a name="tooclone-a-volume"></a>tooclone en volym
1. På hello StorSimple Manager-tjänsten klickar du på hello **säkerhetskopieringskatalog** fliken och markera en säkerhetskopia.
2. Expandera hello säkerhetskopia tooview hello associerade volymer. Klicka och välj en volym från hello säkerhetskopia.
   
     ![Klona en volym](./media/storsimple-clone-volume/HCS_Clone.png) 
3. Klicka på **klona** toobegin kloning hello valda volymen.
4. Hello klona volymen i guiden under **ange namn och plats**:
   
   1. Identifiera en målenhet. Detta är hello plats där hello klona kommer att skapas. Du kan välja hello samma enhet eller ange en annan enhet. Om du väljer en volym som är associerade med andra leverantörer av molntjänster (inte Azure) hello nedrullningsbara lista för hello målenhet visas endast fysiska enheter. Du kan inte klona en volym som är associerade med andra molntjänstleverantörer på en virtuell enhet.
      
      > [!NOTE]
      > Kontrollera att hello kapacitet som krävs för hello klona är lägre än hello kapacitet på hello målenhet.
      > 
      > 
   2. Ange ett unikt volymnamn för din kloning. hello-namnet måste innehålla mellan 3 och 127 tecken.
   3. Klicka på pilikonen hello ![pilikon](./media/storsimple-clone-volume/HCS_ArrowIcon.png) tooproceed toohello nästa sida.
5. Under **ange värdar som kan använda den här volymen**:
   
   1. Ange en åtkomstkontrollpost (ACR) för hello kloning. Du kan lägga till en ny ACR eller välj hello befintlig lista.
   2. Klicka på kryssikonen hello ![kryssikon](./media/storsimple-clone-volume/HCS_CheckIcon.png)toocomplete hello-åtgärden.
6. Ett jobb för klon initieras och du meddelas när hello klona har skapats. Klicka på **visa jobb** toomonitor hello klona jobb på hello **jobb** sidan.
7. Efter hello har klona jobbet slutförts:
   
   1. Gå toohello **enheter** sida och välj hello **Volymbehållare** fliken. 
   2. Välj hello volymbehållare som är associerad med hello källvolymen som du har klonat. I hello lista över volymer, bör du se hello klon som nyss skapades.

> [!NOTE]
> Övervakning och standard säkerhetskopieringen inaktiveras automatiskt på en klonade volym.
> 
> 

En klon som har skapats på detta sätt är ett tillfälligt kloning. Mer information om kloning typer finns [tillfälliga och permanenta kloner](#transient-vs-permanent-clones).

Den här klonen är nu en vanlig volym och åtgärder som är möjligt på en volym är tillgänglig för hello kloning. Du behöver tooconfigure volymen för säkerhetskopior.

## <a name="transient-vs-permanent-clones"></a>Tillfälligt kontra permanent kloner
Tillfälliga och permanenta kloner skapas bara när du klona på tooa annan enhet. Du kan också klona en viss volym från en säkerhetskopia tooa annan enhet. En klon som skapats i det här sättet är en *tillfälligt* kloning. hello tillfälligt klona har referenser toohello ursprungliga volymen och använder den volymen tooread vid skrivning till lokalt. 

När du har tagit en ögonblicksbild i molnet med tillfälliga klonen hello resulterande klona blir en *permanent* kloning. hello permanent klona är oberoende och har inte några referenser toohello ursprungliga volymen som den har klonats från.  

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenarier för tillfälliga och permanenta kloner
hello följande avsnitt beskrivs exempel situationer där tillfälliga och permanenta kloner kan användas.

### <a name="item-level-recovery-with-a-transient-clone"></a>Återställning på objektnivå med en tillfälliga klon
Du måste toorecover en en-år-gamla Microsoft PowerPoint-fil. IT-administratören identifierar hello säkerhetskopieringen från den aktuella tidsperioden och sedan filter hello volym. Hej administratör sedan klonar volymen hello hittar hello-fil som du söker efter och ger den tooyou. I det här scenariot används ett tillfälligt kloning. 

![Video tillgänglig](./media/storsimple-clone-volume/Video_icon.png) **Video tillgänglig**

toowatch en video som visar hur du kan använda hello klona och återställa funktioner i StorSimple toorecover borttagna filer, klickar du på [här](https://azure.microsoft.com/documentation/videos/storsimple-recover-deleted-files-with-storsimple/).

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Testa i produktionsmiljö för hello med en permanent klon
Du måste tooverify ett tester programfel i hello-produktionsmiljö. Du kan skapa en klon av hello volym i produktionsmiljö hello genom att ta en ögonblicksbild i molnet för den här klonen. hello är klonade volymen nu oberoende. I det här scenariot används en permanent kloning.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[återställer en StorSimple-volym från en säkerhetskopia](storsimple-restore-from-backup-set.md).
* Lär dig hur för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md).

