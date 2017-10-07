---
title: "aaaClone en volym på StorSimple 8000-serien | Microsoft Docs"
description: "Beskriver hello klona av olika typer och användning och förklarar hur du kan använda en säkerhetskopia tooclone en enskild volym på en enhet för StorSimple 8000-serien."
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
ms.date: 07/26/2017
ms.author: alkohli
ms.openlocfilehash: 4f7e1f62f17c7b2bd72820a00a5ab87b7e192332
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hello-storsimple-device-manager-service-in-azure-portal-tooclone-a-volume"></a>Använd hello StorSimple enheten Manager-tjänsten i Azure portal tooclone en volym

## <a name="overview"></a>Översikt

Den här självstudiekursen beskriver hur du kan använda en säkerhetskopia tooclone en enskild volym via hello **säkerhetskopieringskatalog** bladet. Här förklaras också hello skillnaden mellan *tillfälligt* och *permanent* klonar. hello anvisningarna i den här kursen gäller tooall hello StorSimple 8000-serieenhet som kör uppdatering 3 eller senare.

Hej StorSimple Enhetshanteraren service **säkerhetskopieringskatalog** bladet visar alla hello säkerhetskopior som skapas när manuell eller automatisk säkerhetskopiering utförs. Du kan sedan välja en volym i en säkerhetskopia tooclone.

 ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/bucatalog.png)

## <a name="considerations-for-cloning-a-volume"></a>Överväganden för kloning av en volym

Överväg att hello följande information när du klonar en volym.

- En klon fungerar i hello samma sätt som en vanlig volym. Alla åtgärder som är möjligt på en volym är tillgänglig för hello kloning.

- Övervakning och standard säkerhetskopieringen inaktiveras automatiskt på en klonade volym. Du behöver tooconfigure klonade volym för säkerhetskopior.

- En lokalt Fäst volym klonas som en nivåindelad volym. Om du behöver hello klonade volym toobe lokalt Fäst, kan du konvertera hello klona tooa lokalt Fäst volym när hello kopieringen har slutförts. För information om hur du konverterar en nivåindelad volym tooa lokalt Fäst volym, gå för[ändra hello volymtyp](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).

- Om du försöker tooconvert en klonade volym från nivåindelade toolocally Fäst omedelbart efter att Kloningen (när det är fortfarande ett tillfälligt klona), misslyckas hello konvertering med hello följande felmeddelande:

    `Unable toomodify hello usage type for volume {0}. This can happen if hello volume being modified is a transient clone and hasn’t been made permanent. Take a cloud snapshot of this volume and then retry hello modify operation.`

    Det här felmeddelandet endast om du klona på tooa annan enhet. Du konverterar hello volym toolocally Fäst om du först konverterar hello tillfälligt klona tooa permanent kloning. Ta en ögonblicksbild i molnet av hello tillfälligt klona tooconvert den tooa permanent kloning.

## <a name="create-a-clone-of-a-volume"></a>Skapa en klon av en volym

Du kan skapa en klon på hello samma enhet, en annan enhet eller även en moln-installation med hjälp av en lokal eller molnbaserade ögonblicksbilder.

hello beskrivs nedan hur toocreate en klon från hello säkerhetskopiera katalog.  En alternativ metod tooinitiate klona är toogo för**volymer**, väljer du en volym, högerklicka på tooinvoke hello snabbmenyn och välj **klona**.

Utför följande steg toocreate en klon av volymen från hello säkerhetskopieringskatalogen hello.

#### <a name="tooclone-a-volume"></a>tooclone en volym

1. Gå tooyour StorSimple enheten Manager-tjänsten och klicka sedan på **säkerhetskopieringskatalog**.

2. Välj en säkerhetskopia på följande sätt:
   
   1. Välj lämplig hello-enhet.
   2. Välj hello volym eller säkerhetskopiering princip för säkerhetskopiering hello tooselect gärna i hello nedrullningsbara listan.
   3. Ange hello tidsintervall.
   4. Klicka på **tillämpa** tooexecute den här frågan.

    hello ska säkerhetskopior som är associerade med principen för hello markerad volym eller säkerhetskopiering visas i hello lista över säkerhetskopior.
   
    ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/bucatalog.png)
     
3. Expandera hello säkerhetskopia tooview hello associerade volymer. Dessa volymer måste vara offline på hello värd och enheten innan du kan återställa dem. Komma åt hello volymer på hello **volymer** bladet för din enhet och hello Följ stegen i [kopplar från en volym](storsimple-8000-manage-volumes-u2.md#take-a-volume-offline) tootake dem offline.
   
   > [!IMPORTANT]
   > Kontrollera att du har vidtagit hello volymer offline på värden för hello först innan du utför hello volymer på hello enhet. Om du inte vidtar hello volymer offline på värden för hello leda potentiellt toodata skadas.
   
4. Gå tillbaka toohello **säkerhetskopieringskatalog** och välj en volym i en säkerhetskopia. Högerklicka och välj sedan hello snabbmenyn **klona**.

   ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol3b.png) 

3. I hello **klona** bladet hello följande steg:
   
    1. Identifiera en målenhet. Detta är hello plats där hello klona kommer att skapas. Du kan välja hello samma enhet eller ange en annan enhet.

      > [!NOTE]
      > Kontrollera att hello kapacitet som krävs för hello klona är lägre än hello kapacitet på hello målenhet.
       
    2. Ange ett unikt volymnamn för din kloning. hello-namnet måste innehålla mellan 3 och 127 tecken.
      
        > [!NOTE]
        > Hej **klona volymen som** fältet kommer att vara **skiktindelade** även om kloning av en lokalt Fäst volym. Du kan inte ändra den här inställningen. Om du behöver hello klonade volym toobe lokalt Fäst samt, kan du konvertera hello klona tooa lokalt Fäst volym när du har skapat hello kloning. För information om hur du konverterar en nivåindelad volym tooa lokalt Fäst volym, gå för[ändra hello volymtyp](storsimple-8000-manage-volumes-u2.md#change-the-volume-type).
          
    3. Under **anslutna värdar**, ange en åtkomstkontrollpost (ACR) för hello kloning. Du kan lägga till en ny ACR eller välj hello befintlig lista. Hej ACR avgör vilka värdar som har åtkomst till den här klonen.
      
        ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol3a.png) 

    4. Klicka på **klona** toocomplete hello igen.

4. Ett jobb för klon initieras och du meddelas när hello klona har skapats. Klicka på meddelandet om hello eller gå för**jobb** bladet toomonitor hello klona jobb.

    ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol5.png)

7. När hello klona jobbet har slutförts, går tooyour enheten och klickar på **volymer**. I hello lista över volymer, bör du se hello klon som just skapades i hello samma volymbehållare som har hello källvolymen.

    ![Säkerhetskopian lista](./media/storsimple-8000-clone-volume-u2/clonevol6.png)

En klon som har skapats på detta sätt är ett tillfälligt kloning. Mer information om kloning typer finns [tillfälliga och permanenta kloner](#transient-vs-permanent-clones).


## <a name="transient-vs-permanent-clones"></a>Tillfälligt kontra permanent kloner
Tillfälligt kloner skapas bara när du klonar tooanother enhet. Du kan också klona en viss volym från en säkerhetskopia tooa annan enhet som hanteras av hello StorSimple Enhetshanteraren. hello tillfälligt klona har referenser toohello data i hello ursprungliga volymen och att data tooread och skriva lokalt på hello målenhet.

När du har tagit en ögonblicksbild i molnet med tillfälliga klonen hello resulterande klon är en *permanent* kloning. Under den här processen skapas på hello molnet en kopia av hello data och hello tid toocopy data bestäms av hello storleken på hello data och hello Azure latens (detta är en kopia av Azure till Azure). Den här processen kan ta tooweeks dagar. hello tillfälligt klona blir en permanent kloning och innehåller inte några referenser toohello ursprungliga volymdata som den har klonats från.

## <a name="scenarios-for-transient-and-permanent-clones"></a>Scenarier för tillfälliga och permanenta kloner
hello följande avsnitt beskrivs exempel situationer där tillfälliga och permanenta kloner kan användas.

### <a name="item-level-recovery-with-a-transient-clone"></a>Återställning på objektnivå med en tillfälliga klon
Du måste toorecover en en-år-gamla Microsoft PowerPoint-fil. IT-administratören identifierar hello säkerhetskopieringen från den tidpunkten och sedan filter hello volym. Hej administratör sedan klonar volymen hello hittar hello-fil som du söker efter och ger den tooyou. I det här scenariot används ett tillfälligt kloning.

### <a name="testing-in-hello-production-environment-with-a-permanent-clone"></a>Testa i produktionsmiljö för hello med en permanent klon
Du måste tooverify ett tester programfel i hello-produktionsmiljö. Du skapar en klon av hello volymen i hello produktionsmiljö och ta en ögonblicksbild i molnet för den här klonen toocreate en oberoende klonade volym. I det här scenariot används en permanent kloning.

## <a name="next-steps"></a>Nästa steg
* Lär dig hur för[återställer en StorSimple-volym från en säkerhetskopia](storsimple-8000-restore-from-backup-set-u2.md).
* Lär dig hur för[Använd hello StorSimple Enhetshanteraren service tooadminister StorSimple-enheten](storsimple-8000-manager-service-administration.md).

