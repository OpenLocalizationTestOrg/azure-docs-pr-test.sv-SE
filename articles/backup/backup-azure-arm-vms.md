---
title: aaaBack virtuella Azure-datorer | Microsoft Docs
description: "Identifiera, registrera och säkerhetskopiera virtuella datorer i Azure tooa recovery services-valvet."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering av virtuella datorer Säkerhetskopiera virtuella; säkerhetskopiering och katastrofåterställning återställning. arm säkerhetskopiering"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a204a42726450a7fd89b5563a786b5070578b113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-tooa-recovery-services-vault"></a>Säkerhetskopiera virtuella datorer i Azure tooa Recovery Services-valvet
> [!div class="op_single_selector"]
> * [Säkerhetskopiera virtuella datorer tooRecovery Services-valvet](backup-azure-arm-vms.md)
> * [Säkerhetskopiera virtuella datorer tooBackup valvet](backup-azure-vms.md)
>
>

Den här artikeln beskrivs hur tooback in virtuella Azure-datorer (både distribuerade Resource Manager och klassisk distribuerade) tooa Recovery Services-valvet. De flesta hello arbete för att säkerhetskopiera virtuella datorer är hello förberedelser. Innan du kan säkerhetskopiera eller mellan en virtuell dator, måste du slutföra hello [krav](backup-azure-arm-vms-prepare.md) tooprepare din miljö för att skydda dina virtuella datorer. När du har slutfört hello krav, kan du initiera hello Säkerhetskopieringsåtgärden tootake ögonblicksbilder av den virtuella datorn.


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

Mer information finns i hello artiklar på [planerar din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md) och [virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/).

## <a name="triggering-hello-backup-job"></a>Utlösande hello säkerhetskopieringsjobb
hello princip för säkerhetskopiering som är associerade med hello Recovery Services-valvet definierar när och hur ofta hello Säkerhetskopieringsåtgärden körs. Som standard är hello första schemalagd säkerhetskopiering hello första säkerhetskopian. Tills hello första säkerhetskopian inträffar hello senaste Status för säkerhetskopiering på hello **säkerhetskopieringsjobb** bladet visas som **varning (första säkerhetskopian väntande)**.

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

Om din första säkerhetskopian förfaller toobegin snart, rekommenderas att du kör **Säkerhetskopiera nu**. hello börjar följande procedur från instrumentpanelen för hello-valvet. Den här proceduren används för att köra hello inledande säkerhetskopieringsjobbet när du har slutfört alla krav. Den här proceduren är inte tillgängligt om hello inledande säkerhetskopieringsjobbet redan har körts. hello anger associerade säkerhetskopieringsprincip hello nästa säkerhetskopieringsjobb.  

toorun hello inledande säkerhetskopieringsjobbet:

1. Hello valvet instrumentpanelen, klicka på hello antalet under **säkerhetskopiering objekt**, eller klicka på hello **säkerhetskopiering objekt** panelen. <br/>
  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)

  Hej **säkerhetskopiering objekt** blad öppnas.

  ![Säkerhetskopieringsobjekt](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. På hello **säkerhetskopiering objekt** bladet, Välj hello-objektet.

  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  Hej **säkerhetskopiering objekt** lista öppnas. <br/>

  ![Säkerhetskopieringsjobbet utlöses](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. På hello **säkerhetskopiering objekt** klickar du på ellipserna hello **...**  tooopen hello snabbmenyn.

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu.png)

  hello snabbmenyn visas.

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. Klicka på hello snabbmenyn **Säkerhetskopiera nu**.

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  hello säkerhetskopiering nu blad öppnas.

  ![Visar hello säkerhetskopiering nu bladet](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. På hello säkerhetskopiering nu bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.

  ![Ange hello sista dag hello säkerhetskopiering nu återställningspunkten behålls](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  Distribution meddelanden att du vet hello jobbet har utlösts och att du kan övervaka hello hello jobb på sidan för hello säkerhetskopiering jobb. Beroende på hello storlek på den virtuella datorn, kan det ta en stund att skapa hello första säkerhetskopian.

6. tooview och spåra hello status för hello första säkerhetskopian på hello valvet instrumentpanelen på hello **säkerhetskopieringsjobb** Klicka på panelen **pågår**.

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  hello säkerhetskopieringsjobb blad öppnas.

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  I hello **säkerhetskopiera jobb** bladet hittar du hello status för alla jobb. Kontrollera om hello säkerhetskopieringsjobbet för den virtuella datorn pågår fortfarande eller om den har slutförts. När ett säkerhetskopieringsjobb är klar är hello status *slutförd*.

  > [!NOTE]
  > Som en del av hello säkerhetskopieringsåtgärd utfärdar hello Azure Backup service kommandot toohello säkerhetskopiering filnamnstillägget i varje VM-tooflush alla skriver och utför en programkonsekvent ögonblicksbild.
  >
  >

## <a name="troubleshooting-errors"></a>Felsökning av fel
Om du stöter på problem under säkerhetskopieringen för den virtuella datorn, se hello [VM felsökningsartikel](backup-azure-vms-troubleshoot.md) för hjälp.

## <a name="next-steps"></a>Nästa steg
Nu när du har skyddat den virtuella datorn finns hello följande artiklar toolearn om VM hanteringsuppgifter och hur toorestore virtuella datorer.

* [Hantera och övervaka dina virtuella datorer](backup-azure-manage-vms.md)
* [Återställa virtuella datorer](backup-azure-arm-restore-vms.md)
