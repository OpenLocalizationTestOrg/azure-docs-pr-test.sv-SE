---
title: "aaaUse a Windows VM felsökning i hello Azure-portalen | Microsoft Docs"
description: "Lär dig hur tootroubleshoot virtuella problem med Windows i Azure med anslutande hello OS disk tooa recovery VM hello Azure-portalen"
services: virtual-machines-windows
documentationCenter: 
authors: genlin
manager: timlt
editor: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/26/2017
ms.author: genli
ms.openlocfilehash: 396f70338fa39f80bb9adcb9244d3c83f2a233eb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-portal"></a>Felsöka en virtuell Windows-dator genom att koppla hello OS tooa diskåterställning VM med hjälp av hello Azure-portalen
Om din Windows-dator (VM) i Azure påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv. Ett vanligt exempel är en programuppdatering för misslyckade som förhindrar hello VM kan tooboot har. Det här artikeln beskriver hur toouse hello Azure portal tooconnect din virtuella hårddisk disk tooanother Windows VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.

## <a name="recovery-process-overview"></a>Översikt över återställningsprocessen
hello felsökningsprocessen är följande:

1. Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.
2. Anslut och montera hello virtuell hårddisk tooanother Windows VM i felsökningssyfte.
3. Ansluta toohello felsökning VM. Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.
4. Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.
5. Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.


## <a name="determine-boot-issues"></a>Fastställa startproblem
toodetermine till den virtuella datorn inte är kan tooboot korrekt, granska hello startdiagnostikinställningar VM skärmbild. Ett vanligt exempel skulle vara en programuppdatering för misslyckade eller en underliggande virtuell hårddisk som tagits bort eller flyttats.

Välj den virtuella datorn i hello-portalen och bläddrar sedan nedåt toohello **stöd + felsökning** avsnitt. Klicka på **starta diagnostik** tooview hello skärmbild. Notera eventuella felmeddelanden eller felkoder toohelp avgöra varför hello VM har uppstått ett problem. hello visas följande exempel en virtuell dator som väntar på att stoppa tjänster:

![Visa VM konsolen startdiagnostikinställningar loggar](./media/troubleshoot-recovery-disks-portal/screenshot-error.png)

Du kan också klicka på **skärmbild** toodownload en avbildning av hello VM skärmbild.


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

1. Öppna en anslutning till Fjärrskrivbord anslutning tooyour VM. Välj den virtuella datorn i hello portal och klicka på **Anslut**. Ladda ned och öppna hello RDP-anslutningsfil. Ange dina autentiseringsuppgifter toolog i tooyour VM enligt följande:

    ![Logga in tooyour VM med hjälp av fjärrskrivbord](./media/troubleshoot-recovery-disks-portal/open-remote-desktop.png)

2. Öppna **Serverhanteraren**och välj **fil- och lagringstjänster**. 

    ![Välj fil- och lagringstjänster i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

3. hello datadisk identifieras automatiskt och ansluten. toosee en lista över hello-anslutna diskar, Välj **diskar**. Du kan välja din data tooview diskvolyminformation, inklusive hello enhetsbeteckning. Hej följande exempel visar hello datadisk som är ansluten och använda **F:**:

    ![Disk som är ansluten och volyminformation i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-disk-attached.png)


## <a name="fix-issues-on-original-virtual-hard-disk"></a>Åtgärda problemen på den ursprungliga virtuella hårddisken
Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov. När du har åtgärdat hello problem, fortsätter du med hello följande steg.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Demontera och koppla från den ursprungliga virtuella hårddisken
Koppla bort hello befintlig virtuell hårddisk från den virtuella datorn med felsökning när din fel har åtgärdats. Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.

1. Hello RDP-session tooyour VM, öppna **Serverhanteraren**och välj **fil- och lagringstjänster**:

    ![Välj fil- och lagringstjänster i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-select-storage.png)

2. Välj **diskar** och välj din datadisk. Högerklicka på din datadisk och välj **koppla från**:

    ![Ange hello datadisk som offline i Serverhanteraren](./media/troubleshoot-recovery-disks-portal/server-manager-set-disk-offline.png)

3. Nu ska du koppla från hello virtuella hårddiskarna från hello VM. Välj den virtuella datorn i hello Azure-portalen och klicka på **diskar**. Välj en befintlig virtuell hårddisk och klicka sedan på **Detach**:

    ![Koppla från en befintlig virtuell hårddisk](./media/troubleshoot-recovery-disks-portal/detach-disk.png)

    Vänta tills hello VM har disk har kopplats bort hello data innan du fortsätter.

## <a name="create-vm-from-original-hard-disk"></a>Skapa virtuell dator från den ursprungliga hårddisken
toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot. Klicka på hello **distribuera tooAzure** knappen på följande sätt:

![Distribuera virtuell dator från mall från Github](./media/troubleshoot-recovery-disks-portal/deploy-template-from-github.png)

hello mallen läses in i hello Azure-portalen för distribution. Ange hello namn för din nya virtuella datorn och befintliga Azure-resurser och klistra in hello URL tooyour befintlig virtuell hårddisk. toobegin hello distributionen, klickar du på **inköp**:

![Distribuera virtuell dator från mall](./media/troubleshoot-recovery-disks-portal/deploy-from-image.png)


## <a name="re-enable-boot-diagnostics"></a>Återaktivera startdiagnostikinställningar
När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras. toocheck hello status för startdiagnostikinställningar och starta om det behövs, Välj den virtuella datorn i hello-portalen. Under **övervakning**, klickar du på **diagnostikinställningarna**. Kontrollera status för hello är **på**, och hello bockmarkering bredvid för**starta diagnostik** är markerad. Om du gör några ändringar klickar du på **spara**:

![Uppdatera startdiagnostikinställningar](./media/troubleshoot-recovery-disks-portal/reenable-boot-diagnostics.png)

## <a name="next-steps"></a>Nästa steg
Om du har problem med anslutning tooyour VM finns [felsökning av RDP-anslutningar tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Windows-VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).