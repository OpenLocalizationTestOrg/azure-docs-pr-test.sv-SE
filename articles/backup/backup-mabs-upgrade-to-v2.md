---
title: aaaInstall Azure Backup Server v2 | Microsoft Docs
description: "Azure Backup-Server v2 ger förbättrad säkerhetskopiering för att skydda virtuella datorer, filer och mappar, arbetsbelastningar och mer. Lär dig hur tooinstall eller uppgradera tooAzure Backup Server v2."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: masaran;markgal
ms.openlocfilehash: 5b1699dadd3a173f1c0ef91a1a600bc5e12f20ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="install-azure-backup-server-v2"></a>Installera Azure Backup-Server v2

Azure Backup-Server skyddar dina virtuella datorer (VM), arbetsbelastningar, filer och mappar och mer. Azure Backup Server v2 bygger på Azure Backup Server v1 och innehåller nya funktioner som inte är tillgängliga i v1. En jämförelse av funktioner mellan v1 och v2 finns [Azure Backup Server protection matrisen](backup-mabs-protection-matrix.md). 

hello ytterligare funktioner i Backup Server v2 är en uppgradering från Backup Server v1. Backup Server v1 är dock inte ett krav för att installera Backup Server v2. Om du vill tooupgrade från Backup Server v1 tooBackup v2 för Server, installera Backup Server v2 på hello Backup Server protection-server. De befintliga inställningarna för säkerhetskopiering förblir intakta.

Du kan installera Backup Server v2 på Windows Server 2012 R2 eller Windows Server 2016. tootake nytta av nya funktioner som System Center 2016 Data Protection Manager Modern Backup Storage, måste du installera Backup Server v2 på Windows Server 2016. Innan du uppgraderar tooor installera Backup Server v2 Läs om hello [installationskrav](https://docs.microsoft.com/system-center/dpm/install-dpm#setup-prerequisites).

> [!NOTE]
> Azure Backup-Server har hello samma kodbas som System Center Data Protection Manager. Backup Server v1 är likvärdiga tooData Protection Manager 2012 R2 och Backup Server v2 är likvärdiga tooData Protection Manager 2016. Ibland refererar hello Data Protection Manager-dokumentationen till den här artikeln.
>
>

## <a name="upgrade-backup-server-toov2"></a>Uppgradera reservserver toov2
tooupgrade från Backup Server v1 tooBackup Server v2, kontrollera att installationen har hello krävs uppdateringar:

- [Uppdatera skyddsagenter hello](backup-mabs-upgrade-to-v2.md#update-the-dpm-protection-agent) på hello skyddade servrar.
- Uppgradera Windows Server 2012 R2 tooWindows Server 2016.
- Uppgradera Azure Backup Server fjärradministratör på alla produktionsservrar.
- Se till att säkerhetskopieringen angetts toocontinue utan att starta om produktionsservern för.


### <a name="upgrade-steps-for-backup-server-v2"></a>Uppgradering för säkerhetskopiering v2

1. I hello Download Center [hämta hello uppgradera installer](https://go.microsoft.com/fwlink/?LinkId=626082).

2. När du har extraherat hello installationsguiden Se **köra setup.exe** är markerad och välj sedan **Slutför**.

  ![Installationsprogram - konfigurationen](./media/backup-mabs-upgrade-to-v2/run-setup.png)

3. Hello i guiden för Microsoft Azure Backup Server under **installera**väljer **Microsoft Azure Backup Server**.

  ![Installationsprogram - väljer installera](./media/backup-mabs-upgrade-to-v2/mabs-installer-s1.png)

4. På hello **Välkommen** granskar hello varningar, och välj sedan **nästa**.

  ![Installationsprogram – välkomstsida](./media/backup-mabs-upgrade-to-v2/mabs-installer-s2.png)

5. Installationsguiden för hello utför kravkontroller toomake att uppgradera din miljö. På hello **nödvändiga kontrollerar** väljer **Kontrollera**.

  ![Installationsprogram - nödvändiga kontrollerar sida](./media/backup-mabs-upgrade-to-v2/mabs-installer-s3-perform-checks.png)

6. Din miljö måste klara hello nödvändiga kontroller. Om din miljö inte klarar hello kontroller, notera hello problem och åtgärda dem. Markera **kontrollen igen**. När du skickar hello nödvändiga kontroller, Välj **nästa**.

  ![Installationsprogram - knappen Kontrollera igen](./media/backup-mabs-upgrade-to-v2/mabs-installer-s4-pass-checks.png)

7. På hello **SQL-inställningar** väljer hello önskat alternativ för SQL-installationen, och välj sedan **kontrollera och installera**.

  ![Installationsprogram - sidan SQL-inställningar](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5-sql-settings.png)

  hello kontroller kan ta några minuter. När hello kontroller är klar, Välj **nästa**.

  ![Installationsprogram – kontrollera om SQL-inställningar och installera](./media/backup-mabs-upgrade-to-v2/mabs-installer-s5a-check-and fix-settings.png)

8. På hello **installationsinställningar** gör eventuella ändringar toohello plats där Backup Server är installerat eller toohello Scratch plats. Välj **nästa**.

  ![Installationsprogram - installationsinställningssidan](./media/backup-mabs-upgrade-to-v2/mabs-installer-s6-installation-settings.png)

9. toofinish hello installationsguiden väljer **Slutför**.

  ![Installationsprogram - Slutför](./media/backup-mabs-upgrade-to-v2/run-setup.png)



## <a name="add-storage-for-modern-backup-storage"></a>Lägga till lagringsenheter till moderna Backup Storage

tooimprove säkerhetskopieringslagring effektivitet, reservserver v2 lägger till stöd för volymer. Som reservserver v1, v2 Backup Server har stöd för diskar.

### <a name="add-volumes-and-disks"></a>Lägg till volymer och diskar
Om du kör reservserver v2 på Windows Server 2016 kan du använda volymer toostore säkerhetskopierade data. Volymer ger lagringsbesparingar och snabbare säkerhetskopieringar. Eftersom volymer är ny tooBackup Server, måste du lägga till dem. 

När du lägger till en volym tooBackup Server kan du ge hello volym ett eget namn. Klicka på hello **eget namn** kolumnen hello volym som du vill tooname. Du kan ändra namnet hello senare, om det behövs. Du kan också använda PowerShell tooadd eller ändra eget namn för volymer.

tooadd en volym i hello-administratörskonsolen:

1. Välj i hello Azure Backup Server-administratörskonsolen, **Management** > **disklagring** > **Lägg till**.

    ![Öppna hello lägger till disklagring guiden](./media//backup-mabs-upgrade-to-v2/open-add-disk-storage-wizard.png)

    Hello lägger till disklagring guiden öppnas.

2. På hello **lägger till disklagring** i hello sidan **tillgängliga volymer** rutan, Välj en volym och välj sedan **Lägg till**.
3. I hello **valda volymer** , ange ett eget namn för hello volymen och välj sedan **OK**.

      ![Disklagring guiden Lägg till – Lägg till volym](./media/backup-mabs-upgrade-to-v2/add-volume.png)

  Om du vill tooadd en disk, tillhör hello disken tooa skyddsgrupp som har äldre lagring. Diskarna kan endast användas för dessa skyddsgrupper. Om servern för säkerhetskopiering inte har datakällor som har äldre skydd, finns hello disk inte.

  Läs mer om att lägga till diskar i [lägga till diskar tooincrease äldre lagring](http://docs.microsoft.com/system-center/dpm/upgrade-to-dpm-2016#adding-disks-to-increase-legacy-storage). Du kan inte ge en disk ett eget namn.


### <a name="assign-workloads-toovolumes"></a>Tilldela arbetsbelastningar toovolumes

I Backup Server anger du vilka arbetsbelastningar tilldelas toowhich volymer. Exempelvis kan du ange dyra volymer som stöder ett stort antal i/o-åtgärder per andra (IOPS) toostore endast arbetsbelastningar som kräver ofta, högvolyms-säkerhetskopieringar. Ett exempel är SQL Server med transaktionsloggar.

#### <a name="update-dpmdiskstorage"></a>Uppdatera DPMDiskStorage

tooupdate hello egenskaper för en volym i lagringspoolen för hello i Backup Server, Använd hello PowerShell-cmdlet Update DPMDiskStorage.

Syntax:

`Parameter Set: Volume`

```
Update-DPMDiskStorage [-Volume] <Volume> [[-FriendlyName] <String> ] [[-DatasourceType] <VolumeTag[]> ] [-Confirm] [-WhatIf] [ <CommonParameters>]
```

Alla ändringar du gör med hjälp av PowerShell återspeglas i hello Användargränssnittet.


## <a name="protect-data-sources"></a>Skydda datakällor
toobegin skyddar datakällor, skapar en skyddsgrupp. hello följande steg Markera ändringar eller tillägg toohello guiden Ny Skyddsgrupp.

toocreate en skyddsgrupp:

1. I hello Backup Server-administratörskonsolen, Välj **skydd**.

2. Markera på verktygsfliken hello **ny**.

    Hello Skapa ny Skyddsgrupp guiden öppnas.

  ![Guiden Skapa ny Skyddsgrupp](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-1.png)

3. På hello **Välkommen** väljer **nästa**.
4. På hello **Välj typ av Skyddsgrupp** väljer hello typ av skyddsgrupp du vill toocreate och välj sedan **nästa**.

  ![Sidan Välj Skyddsgruppen](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-2.png)

5. På hello **Välj gruppmedlemmar** i hello sidan **tillgängliga medlemmar** rutan, hello medlemmar med protection Agent finns. I det här exemplet väljer du volym D:\ och E:\ och Lägg till dem toohello **markerade medlemmar** fönstret. Välj **nästa**.

  ![Välj grupp för medlemmar sida](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-3.png)

6. På hello **Välj Dataskyddsmetod** anger en **skyddsgruppnamn**, Välj hello skyddsmetod och välj sedan **nästa**. Om du vill ha kortvarigt skydd, måste du välja hello **Disk** metoden att säkerhetskopiera.

  ![Välj Dataskyddsmetod sida](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-4.png)

7. På hello **ange kortvariga mål** sidan Välj hello information för **Kvarhållningsintervall** och **Synkroniseringsfrekvens**. Markera **nästa**. Du kan också toochange hello schema för när återställningspunkter kan vidtas, Välj **ändra**.

  ![Ange kortvariga mål-sida](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-5.png)

8. På hello **granska Disklagringstilldelning** granskar information om hello datakällor som du har valt, storlek och värden för hello utrymme toobe etablerad och hello lagring målvolymen.

  ![Granska Disklagringstilldelning sidan](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-6.png)

  Lagringsvolymer baseras på hello arbetsbelastning volym allokering (anges med hjälp av PowerShell) och hello tillgängligt lagringsutrymme. Du kan ändra hello lagringsvolymer genom att välja andra volymer i hello nedrullningsbara menyn. Om du ändrar hello värdet för **Mållagringsenhet**, hello värde för **ledigt lagringsutrymme** ändras dynamiskt tooreflect värden under **ledigt utrymme** och **Underprovisioned utrymme**.

  Om hello datakällor växer som planerat, hello värde för hello **Underprovisioned utrymme** kolumn i **ledigt lagringsutrymme** visar hello mängden ytterligare lagringsutrymme som behövs. Använd värdet toohelp planen lagringsbehoven för smooth säkerhetskopieringar. Om hello värdet noll, finns det inga problem med lagring i hello förutsägbar framtid. Om hello värde är ett tal som inte är noll, du har inte tillräckligt lagringsutrymme som allokerats (baserat på din skydd principen och hello datastorleken för dina skyddade medlemmar).

  ![Underbelagd disklagring](./media/backup-mabs-upgrade-to-v2/create-a-protection-group-7.png)

   Skapa din grupp, fullständig hello skyddsguiden toofinish.

## <a name="migrate-legacy-storage-toomodern-backup-storage"></a>Migrera äldre lagring tooModern Backup Storage
När du har uppgraderat tooor installera Backup Server v2 och uppgradera hello operativsystemet tooWindows Server 2016 uppdatera ditt skydd grupper toouse moderna Backup Storage. Som standard ändras inte skyddsgrupper. De fortsätta toofunction som de ursprungligen har ställts in. 

Uppdatera skydd grupper toouse moderna Backup Storage är valfria. tooupdate hello skyddsgrupp, avbryter du skyddet för alla datakällor med hjälp av hello behålla dataalternativet. Lägg sedan till hello data sources tooa ny skyddsgrupp.

1. Välj hello i hello-administratörskonsolen, **skydd** funktion. I hello **Skyddsgruppsmedlem** högerklickar hello medlem, och välj sedan **stoppa skyddet av medlem**.

  ![Stoppa skyddet av medlem](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-stop-protection1.png)

2. I hello **ta bort från gruppen** dialogrutan, granska hello används diskutrymme och hello ledigt utrymme för hello lagringspoolen. hello standard är tooleave hello Återställningspunkter på disk hello och låta dem tooexpire per deras associerade bevarandeprincip. Klicka på **OK**.

  Om du vill tooimmediately returnerade hello används utrymme toohello ledigt disklagringspoolen markerar du hello **ta bort replik på disk** kryssrutan toodelete hello säkerhetskopierade data (och återställningspunkter) som är associerade med medlemmen.

  ![Ta bort från dialogrutan grupp](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-retain-data.png)

3. Skapa en skyddsgrupp som använder Modern Backup Storage. Inkludera hello oskyddad datakällor.


## <a name="add-disks-tooincrease-legacy-storage"></a>Lägga till diskar tooincrease äldre lagringsutrymme

Om du vill toouse äldre lagring med Backup Server kan behöva du tooadd diskar tooincrease äldre lagring. 

tooadd disklagring:

1. Välj i hello-administratörskonsolen, **Management** > **disklagring** > **Lägg till**.

    ![Disklagring dialogrutan Lägg till](http://docs.microsoft.com/system-center/dpm/media/upgrade-to-dpm-2016/dpm-2016-add-disk-storage.png)

4. I hello **lägger till disklagring** markerar **lägga till diskar**.

5. Välj hello diskar som du vill tooadd, väljer i hello lista över tillgängliga diskar, **Lägg till**, och välj sedan **OK**.

## <a name="update-hello-data-protection-manager-protection-agent"></a>Uppdatera hello Data Protection Manager protection agent

Backup-servern använder hello System Center Data Protection Manager protection agent för uppdateringar. Om du uppgraderar en skyddsagent som inte är anslutna toohello nätverk, kan du inte använda hello Data Protection Manager Administrator Console toocomplete en ansluten agent-uppgradering. Du måste uppgradera hello skyddsagenten i en domänmiljö som inaktiva. Tills hello klientdatorn är anslutna toohello nätverk, väntar hello Data Protection Manager-administratörskonsolen visar hello protection agent uppdateringen.

hello följande avsnitt beskrivs hur tooupdate skyddsagenter för klientdatorer som är anslutna och klientdatorer som inte är anslutna.

### <a name="update-a-protection-agent-for-a-connected-client-computer"></a>Uppdatera en skyddsagent för en ansluten klientdator

1. I hello Backup Server-administratörskonsolen, Välj **Management** > **agenter**.

2. Välj hello klientdatorer som du vill tooupdate hello-skyddsagenten i hello visningsfönstret.

  > [!NOTE]
  > Hej **Agentuppdateringar** kolumnen visas när en uppdatering till skyddsagenten är tillgänglig för varje skyddad dator. I hello **åtgärder** rutan, hello **uppdatering** åtgärden är tillgänglig endast när en skyddad dator är markerad och uppdateringar är tillgängliga.
  >
  >

3. tooinstall uppdateras skyddsagenter på hello valda datorer i hello **åtgärder** väljer **uppdatering**.

### <a name="update-a-protection-agent-on-a-client-computer-that-is-not-connected"></a>Uppdatera en skyddsagent på en klientdator som inte är ansluten

1. I hello Backup Server-administratörskonsolen, Välj **Management** > **agenter**.

2. Välj hello klientdatorer som du vill tooupdate hello-skyddsagenten i hello visningsfönstret.

  > [!NOTE]
   > Hej **Agentuppdateringar** kolumnen visas när en uppdatering till skyddsagenten är tillgänglig för varje skyddad dator. I hello **åtgärder** rutan, hello **uppdatering** åtgärden är inte tillgänglig när en skyddad dator markeras om det finns uppdateringar.
  >
  >

3. tooinstall uppdateras skyddsagenter på hello valda datorer, Välj **uppdatering**.

4. För en klientdator som inte är anslutna till toohello, tills hello datorn är ansluten toohello nätverk, hello **Agentstatus** kolumnen visar statusen **väntande uppdatering**.

  När en klientdator är ansluten toohello nätverk, hello **Agentuppdateringar** kolumn för hello klientdator visar statusen **uppdatering**.
  
### <a name="move-legacy-protection-groups-from-old-version-and-sync-hello-new-version-with-azure"></a>Flytta äldre skyddsgrupper från gamla och nya sync hello-versionen med Azure

När både Azure Backup Server och hello OS uppdateras, är du redo tooprotect nya datakällor med moderna Backup Storage. Men redan skyddade datakällor fortsätter toobe skyddas i hello äldre sätt som de befann sig i Azure Backup Server men alla nytt skydd med moderna Backup Storage.

Är toomigrate datakällor från bakåtkompatibelt läge för skydd tooModern säkerhetskopieringslagring nedanstående steg.

• Lägg till hello nya volymerna toohello DPM-lagringspoolen och tilldela eget namn och data källkod om så önskas.
• För varje datakälla som är i bakåtkompatibelt läge, Avbryt skyddet av hello datakällor och ”Kvarhåll skyddade Data”.  Detta gör att återställning av den äldre återställningspunkter efter migreringen.

• Skapa en ny sida och välj hello datakällor som är toobe lagras med nya format.
• DPM gör en kopia av replik från hello äldre säkerhetskopieringslagring till hello moderna säkerhetskopiering lagringsvolym lokalt.
Obs: Detta visas som en återställning efter jobb • alla nya synkronisering och återställningspunkter sparas i den moderna Backup Storage.
• Gamla återställningspunkterna rensas ut eftersom de går ut och slutligen Frigör hello diskutrymme.
• När alla hello äldre volymer tas bort från hello gamla lagringsenheter, hello disk kan tas bort från Azure backup och hello system.
• Ta en säkerhetskopia av hello Azure DPMDB.

Del 2:-viktiga objekt > Hej nya servern behöver toobe samma namn som hello ursprungliga Azure Backup-server. Du kan inte ändra hello namnet på hello nya Azure backup-server om du vill toouse gamla lagringspoolen och DPMDB tooretain återställningspunkter - måste ha säkerhetskopiering av DPM-databasen behöver återställas toobe

1) Avstängning hello ursprungliga Azure backup-server eller starta hello-överföring.
2) Återställa hello datorkonto i active directory.
3) Installera Server 2016 på den nya datorn och namn som den hello samma datornamn som hello ursprungliga Azure Backup-server.
4) Anslut hello domän
5) Installera Azure Backup server V2 (flytta DPM-lagringspooldiskar från den gamla servern och importera)
6) Återställa hello DPMDB tas från slutet av del 2
7) Bifoga hello lagring från hello ursprungliga reservserver toohello ny server.
8) Från SQL återställa hello DPMDB
9) Admin kommandoraden på nya server cd tooMicrosoft Azure Backup för att installera platsen och bin-mappen

Exempel på sökvägen: C:\windows\system32 > cd ”c:\Program Files\Microsoft Azure Backup\DPM\DPM\bin\
tooAzure säkerhetskopiering kör DPMSYNC-SYNC

10) Kör DPMSYNC-SYNC Obs Om du har lagt till nya diskar toohello DPM lagringspoolen i stället för att flytta hello gamla, kör DPMSYNC - Reallocatereplica

## <a name="new-powershell-cmdlets-in-v2"></a>Nya PowerShell-cmdlets i v2

Två nya cmdlets finns tillgängliga när du installerar Azure Backup Server v2: 
* [Montera DPMRecoveryPoint](https://technet.microsoft.com/library/mt787159.aspx)
* [Demontera DPMRecoveryPoint](https://technet.microsoft.com/library/mt787158.aspx)

## <a name="next-steps"></a>Nästa steg

Lär dig hur tooprepare servern eller börja skydda en arbetsbelastning:
- [Förbereda säkerhetskopiering serverarbetsbelastningar](backup-azure-microsoft-azure-backup.md)
- [Använda Backup Server tooback in en VMware-server](backup-azure-backup-server-vmware.md)
- [Använda Backup Server tooback in SQL Server](backup-azure-sql-mabs.md)
- [Använda moderna lagring för säkerhetskopiering med Backup-Server](backup-mabs-add-storage.md)

