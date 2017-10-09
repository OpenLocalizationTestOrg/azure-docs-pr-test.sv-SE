---
title: "Azure-säkerhetskopiering: Återställa filer och mappar från en virtuell dator i Azure-säkerhetskopiering | Microsoft Docs"
description: "Återställa filer från en återställningspunkt för virtuell Azure-dator"
services: backup
documentationcenter: dev-center-name
author: pvrk
manager: shivamg
keywords: "återställning på objektnivå; filåterställning från Virtuella Azure-säkerhetskopia. återställa filer från Azure VM"
ms.assetid: f1c067a2-4826-4da4-b97a-c5fd6c189a77
ms.service: backup
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/20/2017
ms.author: pullabhk;markgal
ms.openlocfilehash: 1a62a0ed83d61272c032ac0377a54099ed118db4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="recover-files-from-azure-virtual-machine-backup"></a>Återställa filer från en säkerhetskopia av virtuell Azure-dator

Azure backup är hello kapaciteten toorestore [virtuella Azure-datorer och diskar](./backup-azure-arm-restore-vms.md) från Virtuella Azure-säkerhetskopieringar. Nu den här artikeln förklarar hur du kan återställa objekt, till exempel filer och mappar från en virtuell dator i Azure-säkerhetskopiering.

> [!Note]
> Den här funktionen är tillgänglig för Azure virtuella datorer som distribueras med hjälp av hello Resource Manager-modellen och skyddade tooa Recovery services-valvet.
> Filåterställning från en krypterad säkerhetskopiering av VM stöds inte.
>

## <a name="mount-hello-volume-and-copy-files"></a>Montera hello volym och kopiera filer

1. Logga in på hello [Azure-portalen](http://portal.Azure.com). Hitta hello relevanta Recovery services-valvet och hello krävs Säkerhetskopiera objekt.

2. Klicka på hello säkerhetskopieringsobjektet bladet **filåterställning**

    ![Öppna Recovery Services-valvet Säkerhetskopiera objekt](./media/backup-azure-restore-files-from-vm/open-vault-item.png)

    Hej **filåterställning** blad öppnas.

    ![Filen recovery bladet](./media/backup-azure-restore-files-from-vm/file-recovery-blade.png)

3. Från hello **Välj återställningspunkt** nedrullningsbara menyn, Välj hello återställningspunkt som innehåller hello filer du vill. Hello senaste återställningspunkten har redan valts som standard.

4. Klicka på **hämta körbara** (för Windows Azure VM) eller **hämta skriptet** (för Linux virtuella Azure-datorn) toodownload hello programvara när du ska använda toocopy filer från hello återställningspunkt.

  hello körbara filer eller skript skapar en anslutning mellan hello lokala datorn och hello angetts återställningspunkt.

5. Du behöver en lösenord toorun hello hämtat skriptet/körbar fil. Du kan kopiera hello lösenord från hello-portalen med hello kopieringsknappen bredvid hello genereras lösenord

    ![Genererat lösenord](./media/backup-azure-restore-files-from-vm/generated-pswd.png)

6. Kör hello körbara filer eller skript på hello datorn där du vill att toorecover hello filer. Du måste köra den med administratörsbehörighet. Om du kör hello skript på en dator med begränsad åtkomst, se till att det finns åtkomst till:

    - Download.microsoft.com
    - Azure-slutpunkter som används för Virtuella Azure-säkerhetskopieringar
    - utgående port 3260

   För Linux hello skriptet kräver 'Öppna iscsi' och 'lshw' tooconnect toohello återställningspunkt. Om de inte finns på hello datorn där den körs, begär behörighet tooinstall hello relevanta komponenter och installerar dem på medgivande.
   
   Ange hello lösenord kopieras från hello portal när du tillfrågas. När du har angett hello giltigt lösenord ansluter hello skript toohello återställningspunkt.
      
    ![Filen recovery bladet](./media/backup-azure-restore-files-from-vm/executable-output.png)
    
   
   Du kan köra hello skript på en dator som har operativsystemet hello samma (eller kompatibla) som hello säkerhetskopierade VM. Se hello [kompatibel OS tabell](backup-azure-restore-files-from-vm.md#compatible-os) för kompatibla operativsystem. Om hello skyddade virtuella Azure hello datorn använder lagringsutrymmen för Windows (för Windows Azure VM: ar) eller LVM/RAID Arrays(for Linux VMs) och du inte kan köra hello körbara filer eller skript på samma virtuella dator. I stället köra den på en dator med ett kompatibelt operativsystem.

### <a name="compatible-os"></a>Kompatibel OS

#### <a name="for-windows"></a>För Windows

följande tabell visar hello kompatibilitet mellan servern och datorn operativsystem hello. När du återställer filer, kan du återställa filer mellan inkompatibla operativsystem.

|Server-OS | Kompatibel klient-OS  |
| --------------- | ---- |
| Windows Server 2012 R2 | Windows 8.1 |
| Windows Server 2012    | Windows 8  |
| Windows Server 2008 R2 | Windows 7   |

#### <a name="for-linux"></a>För Linux

I Linux, hello grundläggande krav är att hello OS av hello dator där hello skript körs ska ha stöd för hello filsystem för hello filer som finns i hello säkerhetskopierade Linux VM. När du väljer ett skript för dator toorun hello, ska du kontrollera att den har hello kompatibla operativsystem och hello-versioner som anges i hello nedan.

|Linux OS | Versioner  |
| --------------- | ---- |
| Ubuntu | 12.04 och senare |
| CentOS | 6.5 och senare  |
| RHEL | 6.7 och senare |
| Debian | 7 och senare |
| Oracle Linux | 6.4 och senare |

hello skriptet kräver python och bash komponenter tooexecute också och på ett säkert sätt ansluta toohello återställningspunkt.

|Komponent | Version  |
| --------------- | ---- |
| Bash | 4 och senare |
| python | 2.6.6 och senare  |


### <a name="identifying-volumes"></a>Identifiera volymer

#### <a name="for-windows"></a>För Windows

När du kör hello exectuable hello operativsystemet monterar hello nya volymer och tilldelar enhetsbeteckningar. Du kan använda Utforskaren eller Utforskaren toobrowse dessa enheter. hello kanske enhetsbeteckningar toohello volymer inte hello hello den ursprungliga virtuella datorn men hello volymnamn samma bokstäver har bevarats. Till exempel om hello volym på hello ursprungliga virtuella datorn har ”datadisk (E:\)”, att volymen kan kopplas till ”datadisk ('Vilken enhetsbeteckning som är tillgängliga':\) på hello lokala datorn. Bläddra igenom alla volymer som nämns i utdata för hello skriptet tills du hittar din mappen.  
       
   ![Filen recovery bladet](./media/backup-azure-restore-files-from-vm/volumes-attached.png)
           
#### <a name="for-linux"></a>För Linux

I Linux är hello volymer hello återställningspunkt monterade toohello mappen där hello skript körs. hello anslutna diskar, volymer och hello motsvarande montera sökvägar visas därför. Dessa montera sökvägar är synliga toousers med roten åtkomst. Bläddra igenom hello volymer som nämns i utdata för hello-skriptet.

  ![Linux-filen recovery bladet](./media/backup-azure-restore-files-from-vm/linux-mount-paths.png)
  

## <a name="closing-hello-connection"></a>Stänger hello-anslutning

Ta bort (eller demontera) på hello ytterligare enheter när du identifierat hello filer och kopiera dem tooa lokal lagringsplats. toounmount hello enheter på hello **filåterställning** bladet i hello Azure-portalen klickar du på **demontera diskar**.

![Demontera diskar](./media/backup-azure-restore-files-from-vm/unmount-disks3.png)

När hello diskar har demonterats, visas ett meddelande som du kan se den lyckades. Det kan ta några minuter för hello anslutning toorefresh så att du kan ta bort hello diskar.

I Linux, när hello anslutning toohello återställningspunkt är fjärrdisken hello OS inte ta bort hello motsvarande monteringssökvägar automatiskt. Dessa finns som ”rad” volymer och de visas, men ett fel genereras när du åtkomst och skrivning hello-filer. De kan ta manuellt bort. hello skript, när den körs identifierar dessa volymer som befintliga från alla tidigare återställningspunkter och rensar dem vid medgivande.

## <a name="special-configurations"></a>Särskilda konfigurationer

### <a name="dynamic-disks"></a>Dynamiska diskar

Om hello Azure VM som har säkerhetskopierats har volymer som sträcker sig över flera diskar (disklänkande och stripe-volymer) och/eller feltoleranta volymer (volymer speglad eller RAID-5) på dynamiska diskar kan du inte köra hello körbara skript på hello samma virtuella dator. I stället köra hello körbara skript på en dator med ett kompatibelt operativsystem.

### <a name="windows-storage-spaces"></a>Lagringsutrymmen för Windows

Lagringsutrymmen för Windows är en teknik i Windows-lagring som du kan använda toovirtualize lagring. Med lagringsutrymmen för Windows kan du gruppera standarddiskar i lagringspooler och sedan skapa virtuella diskar som kallas lagringsutrymmen, från hello tillgängligt utrymme i dessa lagringspooler.

Om hello Azure VM som har säkerhetskopierats använder lagringsutrymmen i Windows, och du inte köra hello körbara skript på hello samma virtuella dator. I stället köra hello körbara skript på en dator med ett kompatibelt operativsystem.

### <a name="lvmraid-arrays"></a>LVM/RAID-matriser

I Linux, logiska volymer (LVM) och/eller programvara är RAID-matriser används toomanage logiska volymer över flera diskar. Om hello säkerhetskopierade Linux VM använder LVM och/eller RAID-matriser kan du inte köra hello skript på hello samma virtuella dator. I stället köra hello skript på en dator med kompatibelt operativsystem och som har stöd för filsystem hello säkerhetskopierade VM.

hello skriptutdata visar hello LVM och/eller RAID-matriser diskar och volymer hello med hello partitionstypen enligt nedan

   ![Linux LVM utdata-bladet](./media/backup-azure-restore-files-from-vm/linux-LVMOutput.png)
   
hello följande kommandon måste toobe kör genom hello användaren toobring partitionerna online. 

**För LVM partitioner**

```
$ pvs <volume name as shown above in hello script output> 
```
Detta visar hello volym gruppnamn under fysiska enheter.

```
$ lvdisplay <volume-group-name from hello pvs command’s results> 
```
Detta visar alla logiska volymer, namn och sökvägarna i en grupp för volymen.

```
$ mount <LV path> </mountpath>
```
toomount hello logiska volymer toohello sökväg valet.


**För RAID-matriser**

```
$ mdadm –detail –scan
```
Visar information om alla raid-diskar. hello relevanta RAID-disk kommer att visas som`/dev/mdm/<RAID array name in hello backed up VM>`

Kommandot hello montera om hello RAID disk har fysiska volymer.
```
$ mount [RAID Disk Path] [/mounthpath]
```

Om den här RAID-disken har en annan LVM som konfigurerats i hello Följ samma procedur som beskrivs ovan för LVM partitioner med hello volymnamn som hello RAID diskens namn

## <a name="troubleshooting"></a>Felsökning

Kontrollera hello i den följande tabellen för ytterligare information om det uppstår problem vid återställning av filer från hello virtuella datorer.

| Felmeddelande / Scenario | Möjlig orsak | Rekommenderad åtgärd |
| ------------------------ | -------------- | ------------------ |
| Exe utdata: *undantag ansluter toohello mål* |Skript är inte kan tooaccess hello återställningspunkt | Kontrollera om hello datorn uppfyller hello åtkomstkrav som nämns ovan|  
|   Exe utdata: *hello mål har redan loggats in via en ISCSI-session.* |   hello skript redan har körts på samma dator och hello enheter har bifogats hello |   hello volymer hello återställningspunkt har redan bifogats. De kan inte vara monterade med samma enhetsbeteckningar för hello hello ursprungliga virtuella datorn. Bläddra igenom alla tillgängliga hello-volymer i hello Utforskaren för filen |
| Exe utdata: *skriptet är ogiltigt eftersom hello diskar har demonterats via portalen/överskred hello 12 hr gränsen. Hämta ett nytt skript från hello-portalen.* |    hello diskar har demonterats från hello-portalen eller hello 12 hr överskriden |  Den här viss exe nu är ogiltig och kan inte köras. Om du vill tooaccess hello filer för att återställa i tidpunkt, besök hello portalen för en ny exe-filen|
| På hello datorn där hello exe körs: hello nya volymer är inte demonteras när hello dismount trycks ned |    hello ISCSI-initieraren på hello datorn inte svarar/uppdatera anslutningen toohello mål och underhålla hello cache |    Vänta några minuter efter hello dismount knappen är nedtryckt. Om hello nya volymer inte är fortfarande demonteras, bläddra igenom alla hello-volymer. Den här framtvingar hello initieraren toorefresh hello anslutning och hello volymen demonteras med ett felmeddelande som hello disken är inte tillgänglig|
| Exe utdata: skript köras men ”nya volymer ansluten” visas inte i utdata för hello-skriptet |   Detta är ett tillfälligt fel   | hello volymer skulle har redan bifogats. Öppna Utforskaren toobrowse. Om du använder hello samma datorn för att köra skript varje gång, starta hello datorn eventuellt och hello listan ska visas i hello efterföljande exe körs. |
| Linux-specifika: inte tooview hello önskad volymer | hello OS av hello dator där hello skript körs kanske inte kan identifiera hello underliggande filsystem hello säkerhetskopierade VM | Kontrollera är om hello återställningspunkt krascher konsekvent eller filkonsekventa. Om hello konsekvent och kör skript på en annan dator vars operativsystem identifierar hello säkerhetskopierade Virtuella datorns filsystem |
| Windows-specifika: inte tooview hello önskad volymer | hello diskar kopplade men hello volymer har inte konfigurerats | Identifiera hello ytterligare diskar relaterade toohello återställningspunkt från hello disk management skärmen. Om någon av dessa diskar är offline tillstånd försök att göra dem online genom att högerklicka på hello disk och klicka på 'Online'|
