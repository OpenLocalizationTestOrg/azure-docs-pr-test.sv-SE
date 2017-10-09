---
title: "aaaUse en Linux felsökning VM i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Linux virtuella problem med anslutning hello OS disk tooa recovery VM hello Azure-portalen"
services: virtual-machines-linux
documentationCenter: 
authors: iainfoulds
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 11/14/2016
ms.author: iainfou
ms.openlocfilehash: 794daa06d7436215af84a61ab9088524254c47df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a>Felsöka en Linux VM genom att koppla hello OS tooa diskåterställning VM med hjälp av hello Azure-portalen
Om Linux-dator (VM) påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv. Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar hello VM kan tooboot har. Det här artikeln beskriver hur toouse hello Azure portal tooconnect din virtuella hårddisk disk tooanother Linux VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.

## <a name="recovery-process-overview"></a>Översikt över återställningsprocessen
hello felsökningsprocessen är följande:

1. Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.
2. Anslut och montera hello virtuell hårddisk tooanother Linux VM i felsökningssyfte.
3. Ansluta toohello felsökning VM. Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.
4. Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.
5. Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.


## <a name="determine-boot-issues"></a>Fastställa startproblem
Granska hello startdiagnostikinställningar och VM skärmbild toodetermine till den virtuella datorn inte är kan tooboot korrekt. Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller en underliggande virtuell hårddisk som tagits bort eller flyttats.

Välj den virtuella datorn i hello-portalen och bläddrar sedan nedåt toohello **stöd + felsökning** avsnitt. Klicka på **starta diagnostik** tooview hälsningsmeddelande konsolen strömmas från den virtuella datorn. Granska hello konsolloggar toosee om du kan fastställa varför hello VM har uppstått ett problem. hello följande exempel visas en virtuell dator som fastnat i underhållsläge som kräver manuell interaktion:

![Visa VM konsolen startdiagnostikinställningar loggar](./media/troubleshoot-recovery-disks-portal/boot-diagnostics-error.png)

Du kan också klicka på **skärmbild** över hello överkant hello Start diagnostik loggen toodownload en avbildning av hello VM skärmbild.


## <a name="view-existing-virtual-hard-disk-details"></a>Visa information om befintlig virtuell hårddisk
Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD). 

Välj din resursgrupp från hello-portalen och sedan ditt lagringskonto. Klicka på **Blobbar**, som i följande exempel hello:

![Välj storage-blobbar](./media/troubleshoot-recovery-disks-portal/storage-account-overview.png)

Du har vanligtvis en behållare med namnet **virtuella hårddiskar** som lagrar dina virtuella hårddiskar. Välj hello behållaren tooview en lista över virtuella hårddiskar. Obs hello namnet på den virtuella Hårddisken (hello prefix är vanligtvis hello namnet på den virtuella datorn):

![Identifiera VHD i lagringsbehållaren](./media/troubleshoot-recovery-disks-portal/storage-container.png)

Välj en befintlig virtuell hårddisk hello listan och kopiera hello URL: en för användning i hello följande steg:

![Kopiera URL: en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/copy-vhd-url.png)


## <a name="delete-existing-vm"></a>Ta bort en befintlig virtuell dator
Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure. En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras. hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC). Varje virtuell hårddisk har tilldelats när kopplade tooa VM. Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort. hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.

Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv. Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto. Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.

Välj den virtuella datorn i hello portal och klicka sedan på **ta bort**:

![VM-Start diagnostik skärmbild som visar start-fel](./media/troubleshoot-recovery-disks-portal/stop-delete-vm.png)

Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM. hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Bifoga en befintlig virtuell hårddisk tooanother VM
Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte. Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobe kan toobrowse och redigera hello disk innehåll. Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel. Välj eller skapa en annan VM toouse för felsökning.

1. Välj din resursgrupp från hello-portalen, och välj sedan den virtuella datorn med felsökning. Välj **diskar** och klicka sedan på **bifoga befintliga**:

    ![Bifoga den befintliga disken i hello-portalen](./media/troubleshoot-recovery-disks-portal/attach-existing-disk.png)

2. tooselect befintliga virtuella hårddisken, klickar du på **VHD-filen**:

    ![Bläddra till den befintliga virtuella hårddisken](./media/troubleshoot-recovery-disks-portal/select-vhd-location.png)

3. Välj ditt lagringskonto och en behållare och sedan på den befintliga virtuella Hårddisken. Klicka på hello **Välj** knappen tooconfirm ditt val:

    ![Välj din befintliga virtuella hårddisk](./media/troubleshoot-recovery-disks-portal/select-vhd.png)

4. Med den virtuella Hårddisken nu markerade, klickar du på **OK** tooattach hello befintlig virtuell hårddisk:

    ![Bekräfta att bifoga en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/attach-disk-confirm.png)

5. Efter några sekunder hello **diskar** fönstret för den virtuella datorn visas en befintlig virtuell hårddisk ansluten som en datadisk:

    ![Befintlig virtuell hårddisk ansluten som en datadisk](./media/troubleshoot-recovery-disks-portal/attached-disk.png)


## <a name="mount-hello-attached-data-disk"></a>Montera hello bifogade datadisk

> [!NOTE]
> hello följande exempel innehåller information om hello steg som krävs på en Ubuntu VM. Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, hello loggfilens platser och `mount` kommandon kan vara lite annorlunda. Läs toohello dokumentationen för din specifika distro för hello ändringarna i kommandona.

1. SSH tooyour felsökning VM som använder hello rätt autentiseringsuppgifter. Om den här disken är hello första data disk ansluten tooyour felsökning VM, är det sannolikt anslutet för`/dev/sdc`. Använd `dmseg` toolist anslutna diskar:

    ```bash
    dmesg | grep SCSI
    ```
    hello utdata är liknande toohello följande exempel:

    ```bash
    [    0.294784] SCSI subsystem initialized
    [    0.573458] Block layer SCSI generic (bsg) driver version 0.4 loaded (major 252)
    [    7.110271] sd 2:0:0:0: [sda] Attached SCSI disk
    [    8.079653] sd 3:0:1:0: [sdb] Attached SCSI disk
    [ 1828.162306] sd 5:0:0:0: [sdc] Attached SCSI disk
    ```

    I föregående exempel hello, hello OS-disken är på `/dev/sda` och hello diskutrymme för varje virtuell dator är på `/dev/sdb`. Om du har flera datadiskar, bör de visas på `/dev/sdd`, `/dev/sde`och så vidare.

2. Skapa en katalog toomount en befintlig virtuell hårddisk. hello följande exempel skapas en katalog med namnet `troubleshootingdisk`:

    ```bash
    sudo mkdir /mnt/troubleshootingdisk
    ```

3. Om du har flera partitioner i en befintlig virtuell hårddisk montera hello krävs partition. hello följande exempel monterar hello första primära partition på `/dev/sdc1`:

    ```bash
    sudo mount /dev/sdc1 /mnt/troubleshootingdisk
    ```

    > [!NOTE]
    > Det är bra toomount datadiskar på virtuella datorer i Azure med hjälp av hello universellt Unik identifierare (UUID) för hello virtuell hårddisk. Den här korta felsökning scenariot behövs inte montering hello virtuell hårddisk med hello UUID. Men vid normal användning, redigering `/etc/fstab` toomount virtuella hårddiskar med enhetsnamn snarare än UUID kan orsaka hello VM toofail tooboot.


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Åtgärda problemen på den ursprungliga virtuella hårddisken
Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov. När du har åtgärdat hello problem, fortsätter du med hello följande steg.

## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Demontera och koppla från den ursprungliga virtuella hårddisken
Koppla bort hello befintlig virtuell hårddisk från den virtuella datorn med felsökning när din fel har åtgärdats. Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.

1. Demontera från hello SSH-session tooyour felsökning VM, hello befintlig virtuell hårddisk. Ändra utanför hello överordnade katalogen för din monteringspunkt:

    ```bash
    cd /
    ```

    Nu demontera hello befintlig virtuell hårddisk. hello följande exempel demonterar hello enhet på `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nu ska du koppla från hello virtuella hårddiskarna från hello VM. Välj den virtuella datorn i hello portal och klicka på **diskar**. Välj en befintlig virtuell hårddisk och klicka sedan på **Detach**:

    ![Koppla från en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Vänta tills hello VM har disk har kopplats bort hello data innan du fortsätter.

## <a name="create-vm-from-original-hard-disk"></a>Skapa virtuell dator från den ursprungliga hårddisken
toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot. Klicka på hello **distribuera tooAzure** knappen på följande sätt:

![Distribuera virtuell dator från mall från GitHub](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

hello mallen läses in i hello Azure-portalen för distribution. Ange hello namn för din nya virtuella datorn och befintliga Azure-resurser och klistra in hello URL tooyour befintlig virtuell hårddisk. toobegin hello distributionen, klickar du på **inköp**:

![Distribuera virtuell dator från mall](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Återaktivera startdiagnostikinställningar
När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras. toocheck hello status för startdiagnostikinställningar och starta om det behövs, Välj den virtuella datorn i hello-portalen. Under **övervakning**, klickar du på **diagnostikinställningarna**. Kontrollera status för hello är **på**, och hello bockmarkering bredvid för**starta diagnostik** är markerad. Om du gör några ändringar klickar du på **spara**:

![Uppdatera startdiagnostikinställningar](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Nästa steg
Om du har problem med anslutning tooyour VM finns [felsökning av SSH-anslutningar tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
