---
title: "aaaUse en Linux felsöka VM med hello Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Linux VM problem med anslutning hello OS disk tooa recovery VM hello Azure CLI 1.0"
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
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: 398f681d1149299d444fcfdab20737315db02855
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-linux-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-hello-azure-cli-10"></a>Felsöka en Linux VM genom att koppla hello OS tooa diskåterställning VM med hjälp av hello Azure CLI 1.0
Om Linux-dator (VM) påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv. Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab` som förhindrar hello VM kan tooboot har. Det här artikeln beskriver hur toouse hello Azure CLI 1.0 tooconnect din virtuella hårddisk disk tooanother Linux VM toofix eventuella fel och sedan återskapa den ursprungliga virtuella datorn.


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#recovery-process-overview) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](../windows/troubleshoot-recovery-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="recovery-process-overview"></a>Översikt över återställningsprocessen
hello felsökningsprocessen är följande:

1. Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.
2. Anslut och montera hello virtuell hårddisk tooanother Linux VM i felsökningssyfte.
3. Ansluta toohello felsökning VM. Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.
4. Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.
5. Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.

Se till att du har [hello senaste Azure CLI 1.0](../../cli-install-nodejs.md) installerad och loggas och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt parameternamn i hello med egna värden. Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.


## <a name="determine-boot-issues"></a>Fastställa startproblem
Granska hello seriella utdata toodetermine till den virtuella datorn inte är kan tooboot korrekt. Ett vanligt exempel är ett ogiltigt värde i `/etc/fstab`, eller hello underliggande virtuell hårddisk som tagits bort eller flyttats.

hello följande exempel hämtas hello seriella utdata från hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

```azurecli
azure vm get-serial-output --resource-group myResourceGroup --name myVM
```

Granska hello seriella utdata toodetermine varför hello VM inte tooboot. Om hello seriella utdata är inte något indikerar måste du kanske måste tooreview loggfiler i `/var/log` när du har hello virtuell hårddisk ansluten tooa felsökning VM.


## <a name="view-existing-virtual-hard-disk-details"></a>Visa information om befintlig virtuell hårddisk
Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD). 

hello följande exempel hämtar information för hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

```azurecli
azure vm show --resource-group myResourceGroup --name myVM
```

Leta efter `Vhd URI` i hello utdata från hello föregående kommando. hello följande trunkerat exempel visas hello `Vhd URI` på hello sista raden:

```azurecli
info:    Executing command vm show
+ Looking up hello VM "myVM"
+ Looking up hello NIC "myNic"
+ Looking up hello public ip "myPublicIP"
...
data:
data:      OS Disk:
data:        OSType                      :Linux
data:        Name                        :myVM
data:        Caching                     :ReadWrite
data:        CreateOption                :FromImage
data:        Vhd:
data:          Uri                       :https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
```


## <a name="delete-existing-vm"></a>Ta bort en befintlig virtuell dator
Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure. En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras. hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC). Varje virtuell hårddisk har tilldelats när kopplade tooa VM. Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort. hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.

Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv. Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto. Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.

följande exempel tar bort hello hello virtuella datorn med namnet `myVM` från hello resursgrupp med namnet `myResourceGroup`:

```azurecli
azure vm delete --resource-group myResourceGroup --name myVM 
```

Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM. hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Bifoga en befintlig virtuell hårddisk tooanother VM
Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte. Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobrowse och redigera hello disk innehåll. Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel. Välj eller skapa en annan VM toouse för felsökning.

När du ansluter hello befintlig virtuell hårddisk, ange hello URL toohello disk hämtades i föregående hello `azure vm show` kommando. hello följande exempel bifogar en befintlig virtuell hårddisk toohello felsökning virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:

```azurecli
azure vm disk attach --resource-group myResourceGroup --name myVMRecovery \
    --vhd-url https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
```


## <a name="mount-hello-attached-data-disk"></a>Montera hello bifogade datadisk

> [!NOTE]
> hello följande exempel innehåller information om hello steg som krävs på en Ubuntu VM. Om du använder en annan Linux distro, till exempel Red Hat Enterprise Linux eller SUSE, hello loggfilens platser och `mount` kommandon kan vara lite annorlunda. Läs toohello dokumentationen för din specifika distro för hello ändringarna i kommandona.

1. SSH tooyour felsökning VM som använder hello rätt autentiseringsuppgifter. Om den här disken är hello första data disk ansluten tooyour felsökning VM, hello disk sannolikt ansluten för`/dev/sdc`. Använd `dmseg` tooview anslutna diskar:

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
När din fel har åtgärdats, demontera och koppla hello befintlig virtuell hårddisk från den virtuella datorn med felsökning. Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.

1. Demontera från hello SSH-session tooyour felsökning VM, hello befintlig virtuell hårddisk. Ändra utanför hello överordnade katalogen för din monteringspunkt:

    ```bash
    cd /
    ```

    Nu demontera hello befintlig virtuell hårddisk. hello följande exempel demonterar hello enhet på `/dev/sdc1`:

    ```bash
    sudo umount /dev/sdc1
    ```

2. Nu ska du koppla från hello virtuella hårddiskarna från hello VM. Avsluta hello SSH-session tooyour felsökning VM. I hello Azure CLI kopplas första listan hello data diskar tooyour felsökning VM. hello följande exempel visar hello datadiskar kopplade toohello virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:

    ```azurecli
    azure vm disk list --resource-group myResourceGroup --vm-name myVMRecovery
    ```

    Obs hello `Lun` värde för en befintlig virtuell hårddisk. hello visar kommandoutdata i följande exempel hello befintlig virtuell disk som är ansluten på LUN 0:

    ```azurecli
    info:    Executing command vm disk list
    + Looking up hello VM "myVMRecovery"
    data:    Name              Lun  DiskSizeGB  Caching  URI
    data:    ------            ---  ----------  -------  ------------------------------------------------------------------------
    data:    myVM              0                None     https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    info:    vm disk list command OK
    ```

    Koppla från hello datadisk från den virtuella datorn med hjälp av hello tillämpliga `Lun` värde:

    ```azurecli
    azure vm disk detach --resource-group myResourceGroup --vm-name myVMRecovery \
        --lun 0
    ```


## <a name="create-vm-from-original-hard-disk"></a>Skapa virtuell dator från den ursprungliga hårddisken
toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd). hello faktiska JSON-mallen finns på hello följande länk:

- https://Raw.githubusercontent.com/Azure/Azure-Quickstart-Templates/Master/201-VM-Specialized-VHD/azuredeploy.JSON

hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot. hello följande exempel distribuerar hello mallen toohello resursgrupp med namnet `myResourceGroup`:

```azurecli
azure group deployment create --resource-group myResourceGroup --name myDeployment \
    --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd/azuredeploy.json
```

Svaret hello efterfrågar hello mall som namn på virtuell dator (`myDeployedVM` hello följande exempel), OS-typen (`Linux`), och VM-storlek (`Standard_DS1_v2`). Hej `osDiskVhdUri` är hello densamma som används när du ansluter hello befintlig virtuell hårddisk toohello felsökning VM som tidigare. Ett exempel på utdata från kommandot hello och anvisningarna är följande:

```azurecli
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName:  myDeployedVM
osType:  Linux
osDiskVhdUri:  https://mystorageaccount.blob.core.windows.net/vhds/myVM201610292712.vhd
vmSize:  Standard_DS1_v2
existingVirtualNetworkName:  myVnet
existingVirtualNetworkResourceGroup:  myResourceGroup
subnetName:  mySubnet
dnsNameForPublicIP:  mypublicipdeployed
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "mydeployment"
+ Waiting for deployment toocomplete
+
```


## <a name="re-enable-boot-diagnostics"></a>Återaktivera startdiagnostikinställningar

När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras. hello följande exempel aktiveras hello diagnostiska tillägg på hello virtuella datorn med namnet `myDeployedVM` i hello resursgrupp med namnet `myResourceGroup`:

```azurecli
azure vm enable-diag --resource-group myResourceGroup --name myDeployedVM
```

## <a name="next-steps"></a>Nästa steg
Om du har problem med anslutning tooyour VM finns [felsökning av SSH-anslutningar tooan Azure VM](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Linux-VM](../windows/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
