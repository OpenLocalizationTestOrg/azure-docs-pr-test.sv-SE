---
title: aaaDeactivate och ta bort en StorSimple-enhet | Microsoft Docs
description: "Beskriver hur tooremove StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den."
services: storsimple
documentationcenter: 
author: SharS
manager: timlt
editor: 
ms.assetid: 155cda38-c5ae-45dc-b7e8-6444494afc9e
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: anbacker
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed86bcd089aa957128e14b1709c836d938c131a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deactivate-and-delete-a-storsimple-8000-series-device-via-storsimple-manager-service"></a>Inaktivera och ta bort en enhet med StorSimple 8000-serien via StorSimple Manager-tjänsten
## <a name="overview"></a>Översikt
Vill du kanske tootake StorSimple-enheten out-of-service (till exempel om du ersätter eller uppgradera din enhet eller om du inte längre använder StorSimple). Om så är fallet hello behöver toodeactivate hello enheten innan du kan ta bort den. Inaktivera servrarna hello anslutningen mellan hello enheten och hello motsvarande StorSimple Manager-tjänsten. Den här självstudiekursen beskrivs hur tooremove en StorSimple-enhet från tjänsten genom att först inaktivera det och sedan ta bort den. 

När du inaktiverar en enhet kan kommer alla data som lagras lokalt på hello enheten inte längre tillgänglig. Endast hello data som är associerade med hello-enhet som lagrats i hello molnet kan återställas.  

> [!WARNING]
> Inaktivering är en PERMANENT åtgärd och kan inte ångras. En inaktiverad enhet kan inte registreras med hello StorSimple Manager-tjänsten om den inte är först återställa toohello fabriksinställningar. 
> 
> Hej fabriksåterställa processen tar bort alla hello-data som lagras lokalt på din enhet. Det är därför viktigt att ta en ögonblicksbild av alla data i molnet innan du inaktiverar en enhet. Detta gör att du toorecover alla hello data i ett senare skede.
> 
> 

Den här självstudiekursen beskrivs hur du:

* Inaktivera en enhet och ta bort hello data
* Inaktivera en enhet och spara hello data

Här förklaras också hur inaktivera och ta bort fungerar på en virtuell StorSimple-enhet.

> [!NOTE]
> Innan du inaktiverar en StorSimple-enhet i fysiska eller virtuella, se till att toostop eller ta bort klienter och värdar som beror på enheten.
> 
> 

## <a name="deactivate-and-delete-data"></a>Inaktivera och ta bort data
Om du är intresserad av att ta bort hello enheten fullständigt och inte vill att tooretain hello data på hello enhet, slutför du hello följande steg.

#### <a name="toodeactivate-hello-device-and-delete-hello-data"></a>toodeactivate hello enheten och ta bort hello data
1. Tidigare toodeactivating en enhet måste du radera alla hello volym behållare (och hello volymer) som är associerade med hello enhet. Du kan ta bort volymbehållare bara när du har tagit bort hello associerade säkerhetskopior.
2. Inaktivera hello enhet enligt följande:
   
   1. På hello StorSimple Manager-tjänsten **enheter** sidan, Välj hello enhet som du vill toodeactivate och hello längst hello-sidan, klickar du på **inaktivera**.
   2. Ett bekräftelsemeddelande visas. Klicka på **Ja** toocontinue. hello Inaktivera processen startar och ta några minuter toocomplete.
3. Efter inaktiveringen, kan du ta bort hello enheten helt. Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello. hello-tjänsten kan sedan inte längre hantera hello bort enheten. Använd hello följande steg toodelete hello enhet:
   
   1. På hello StorSimple Manager-tjänsten **enheter** väljer du en inaktiverad enhet som du vill toodelete.
   2. Klicka på hello längst ned på sidan hello **ta bort**.
   3. Du uppmanas att bekräfta. Klicka på **Ja** toocontinue.
      
      Det kan ta några minuter för hello enheten toobe tas bort.

## <a name="deactivate-and-retain-data"></a>Inaktivera och behålla data
Om du är intresserad av att ta bort hello enhet men vill tooretain hello data slutför du hello följande steg.

#### <a name="toodeactivate-a-device-and-retain-hello-data"></a>toodeactivate en enhet och spara hello data
1. Inaktivera hello enhet. Alla hello volymbehållare och förblir hello ögonblicksbilder av hello enhet.
   
   1. På hello StorSimple Manager-tjänsten **enheter** sidan, Välj hello enhet som du vill toodeactivate och hello längst hello-sidan, klickar du på **inaktivera**.
   2. Ett bekräftelsemeddelande visas. Klicka på **Ja** toocontinue. hello Inaktivera processen startar och ta några minuter toocomplete.
2. Nu kan du växla över hello volymbehållare och hello associerade ögonblicksbilder. Procedurer finns för[redundans och disaster recovery för din StorSimple-enhet](storsimple-device-failover-disaster-recovery.md).
3. Efter inaktivering och växling vid fel, kan du ta bort hello enhet helt. Om du tar bort en enhet tas den bort från hello lista över enheter anslutna toohello. hello-tjänsten kan sedan inte längre hantera hello bort enheten. Slutför hello följande steg toodelete hello enhet:
   
   1. På hello StorSimple Manager-tjänsten **enheter** väljer du en inaktiverad enhet som du vill toodelete.
   2. Klicka på hello längst ned på sidan hello **ta bort**.
   3. Du uppmanas att bekräfta. Klicka på **Ja** toocontinue.
      
      Det kan ta några minuter för hello enheten toobe tas bort.

## <a name="deactivate-and-delete-a-virtual-device"></a>Inaktivera och ta bort en virtuell enhet
Tar bort hello virtuell dator för en virtuell StorSimple-enhet inaktivering. Du kan sedan ta bort hello virtuella datorn och hello resurserna skapas när den etablerades. När hello virtuella enheten inaktiveras kan den inte återställas tooits tidigare tillstånd. 

Inaktivera resulterar i hello följande åtgärder:

* hello virtuella StorSimple-enheten tas bort.
* Hej OSDisk och Datadiskar som skapats för hello virtuella StorSimple-enheten tas bort.
* hello bevaras värdbaserad tjänst och virtuella nätverk som skapades under etableringen. Om du inte använder dessa enheter, bör du ta bort dem manuellt.
* Molnögonblicksbilder som skapats av hello virtuella StorSimple-enheten bevaras.

## <a name="next-steps"></a>Nästa steg
* toorestore hello inaktiverad enhet toofactory standardinställningar finns för[återställer standardinställningarna för hello enheten toofactory](storsimple-manage-device-controller.md#reset-the-device-to-factory-default-settings).
* För teknisk support [kontaktar Microsoft Support](storsimple-contact-microsoft-support.md).
* toolearn mer om hur toouse hello StorSimple Manager-tjänsten, gå för[Använd hello StorSimple Manager service tooadminister StorSimple-enheten](storsimple-manager-service-administration.md). 

