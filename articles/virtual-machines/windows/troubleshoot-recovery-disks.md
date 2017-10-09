---
title: "aaaUse a Windows felsökning av virtuell dator med Azure PowerShell | Microsoft Docs"
description: "Lär dig hur tootroubleshoot Windows VM problem i Azure genom att ansluta hello OS tooa diskåterställning VM med hjälp av Azure PowerShell"
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
ms.openlocfilehash: 7a6a76f64824fe5d06dc4286cb1d87ab8bb794e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-windows-vm-by-attaching-hello-os-disk-tooa-recovery-vm-using-azure-powershell"></a>Felsöka en virtuell Windows-dator genom att koppla hello OS tooa diskåterställning VM med hjälp av Azure PowerShell
Om din Windows-dator (VM) i Azure påträffar ett fel vid start- eller disk, eventuellt tooperform felsökning i hello virtuell hårddisk sig själv. Ett vanligt exempel är en programuppdatering för misslyckade som förhindrar hello VM kan tooboot har. Den här artikeln information om hur toouse Azure PowerShell tooconnect din virtuella hårddisk tooanother Windows VM toofix eventuella fel sedan återskapa den ursprungliga virtuella datorn.


## <a name="recovery-process-overview"></a>Översikt över återställningsprocessen
hello felsökningsprocessen är följande:

1. Ta bort hello VM uppstått problem, hålla hello virtuella hårddiskar.
2. Anslut och montera hello virtuell hårddisk tooanother Windows VM i felsökningssyfte.
3. Ansluta toohello felsökning VM. Redigera filer eller köra några verktyg toofix problem på hello ursprungliga virtuella hårddisken.
4. Demontera och koppla från hello virtuella hårddiskarna från hello felsökning VM.
5. Skapa en virtuell dator med hello ursprungliga virtuella hårddisken.

Se till att du har [hello senaste Azure PowerShell](/powershell/azure/overview) installerad och loggas i tooyour prenumerationen:

```powershell
Login-AzureRMAccount
```

Följande exempel Ersätt parameternamn i hello med egna värden. Exempel parameternamn inkluderar `myResourceGroup`, `mystorageaccount`, och `myVM`.


## <a name="determine-boot-issues"></a>Fastställa startproblem
Du kan visa en skärmbild av den virtuella datorn i Azure toohelp felsöka startproblem. Den här skärmbilden kan hjälpa dig att identifiera varför en virtuell dator misslyckas tooboot. hello följande exempel hämtas hello skärmbild från hello virtuell Windows-dator med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

```powershell
Get-AzureRmVMBootDiagnosticsData -ResourceGroupName myResourceGroup `
    -Name myVM -Windows -LocalPath C:\Users\ops\
```

Granska hello skärmbild toodetermine varför hello VM inte tooboot. Observera eventuella felmeddelanden eller felkoderna som tillhandahålls.


## <a name="view-existing-virtual-hard-disk-details"></a>Visa information om befintlig virtuell hårddisk
Innan du kan koppla en virtuell hårddisk tooanother VM, måste tooidentify hello namnet på hello virtuell hårddisk (VHD).

hello följande exempel hämtar information för hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Leta efter `Vhd URI` inom hello `StorageProfile` avsnitt från hello utdata i hello föregående kommando. hello följande trunkerat exempel visas hello `Vhd URI` hello utgången av hello kodblock:

```powershell
RequestId                     : 8a134642-2f01-4e08-bb12-d89b5b81a0a0
StatusCode                    : OK
ResourceGroupName             : myResourceGroup
Id                            : /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM
Name                          : myVM
Type                          : Microsoft.Compute/virtualMachines
...
StorageProfile                :
  ImageReference              :
    Publisher                 : MicrosoftWindowsServer
    Offer                     : WindowsServer
    Sku                       : 2016-Datacenter
    Version                   : latest
  OsDisk                      :
    OsType                    : Windows
    Name                      : myVM
    Vhd                       :
      Uri                     : https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd
    Caching                   : ReadWrite
    CreateOption              : FromImage
```


## <a name="delete-existing-vm"></a>Ta bort en befintlig virtuell dator
Virtuella hårddiskar och virtuella datorer är två separata resurser i Azure. En virtuell hårddisk är där hello själva operativsystemet, program och konfigurationer lagras. hello Virtuella datorn är enbart metadata som definierar hello storlek eller plats, och refererar till resurser, till exempel en virtuell hårddisk eller virtuella nätverksgränssnittskortet (NIC). Varje virtuell hårddisk har tilldelats när kopplade tooa VM. Även om datadiskar kan anslutas och oberoende även när hello VM körs, kan inte frånkopplas hello OS-disk om hello Virtuella datorresursen tas bort. hello lån fortsätter tooassociate hello OS-disk med en virtuell dator, även om den virtuella datorn är i tillståndet stoppad och frigjord.

Hej första steg toorecover den virtuella datorn är toodelete hello Virtuella datorresursen sig själv. Ta bort hello VM lämnar hello virtuella hårddiskar i ditt lagringskonto. Efter hello VM tas bort, bifoga hello virtuell hårddisk tooanother VM tootroubleshoot och åtgärda hello-fel.

följande exempel tar bort hello hello virtuella datorn med namnet `myVM` från hello resursgrupp med namnet `myResourceGroup`:

```powershell
Remove-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
```

Vänta tills hello VM har tagits bort innan du kopplar hello virtuell hårddisk tooanother VM. hello lånet på hello virtuell hårddisk som associeras med hello VM måste släppas innan du kan koppla hello virtuell hårddisk tooanother VM toobe.


## <a name="attach-existing-virtual-hard-disk-tooanother-vm"></a>Bifoga en befintlig virtuell hårddisk tooanother VM
Hej bredvid för några steg kan du använda en annan virtuell dator i felsökningssyfte. Du bifogar hello befintlig virtuell hårddisk toothis felsökning VM toobrowse och redigera hello disk innehåll. Den här processen kan toocorrect eventuella fel i programkonfigurationen eller granska ytterligare program eller loggfiler, till exempel. Välj eller skapa en annan VM toouse för felsökning.

När du ansluter hello befintlig virtuell hårddisk, ange hello URL toohello disk hämtades i föregående hello `Get-AzureRmVM` kommando. hello följande exempel bifogar en befintlig virtuell hårddisk toohello felsökning virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
Add-AzureRmVMDataDisk -VM $myVM -CreateOption "Attach" -Name "DataDisk" -DiskSizeInGB $null `
    -VhdUri "https://mystorageaccount.blob.core.windows.net/vhds/myVM.vhd"
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

> [!NOTE]
> Lägga till en disk måste du toospecify hello diskens hello storlek. Som vi bifoga en befintlig disk hello `-DiskSizeInGB` har angetts som `$null`. Det här värdet säkerställer hello datadisk är korrekt ansluten och utan hello behöver toodetermine, som hello true storleken på datadisk.


## <a name="mount-hello-attached-data-disk"></a>Montera hello bifogade datadisk

1. RDP-tooyour felsökning VM som använder hello rätt autentiseringsuppgifter. hello följande exempel hämtar hello RDP-anslutningsfil för hello virtuella datorn med namnet `myVMRecovery` i hello resursgrupp med namnet `myResourceGroup`, och laddar ned för`C:\Users\ops\Documents`”

    ```powershell
    Get-AzureRMRemoteDesktopFile -ResourceGroupName "myResourceGroup" -Name "myVMRecovery" `
        -LocalPath "C:\Users\ops\Documents\myVMRecovery.rdp"
    ```

2. hello datadisk identifieras automatiskt och ansluten. Visa hello lista över anslutna volymer toodetermine hello enhetsbeteckning på följande sätt:

    ```powershell
    Get-Disk
    ```

    hello följande exempel på utdata som visar hello virtuell hårddisk ansluten en disk **2**. (Du kan också använda `Get-Volume` tooview hello enhetsbeteckning):

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Online       127 GB MBR
    ```

## <a name="fix-issues-on-original-virtual-hard-disk"></a>Åtgärda problemen på den ursprungliga virtuella hårddisken
Med hello befintlig virtuell hårddisk monterad, kan du nu utföra eventuella underhåll och felsökning efter behov. När du har åtgärdat hello problem, fortsätter du med hello följande steg.


## <a name="unmount-and-detach-original-virtual-hard-disk"></a>Demontera och koppla från den ursprungliga virtuella hårddisken
När din fel har åtgärdats, demontera och koppla hello befintlig virtuell hårddisk från den virtuella datorn med felsökning. Du kan inte använda din virtuella hårddisk med andra Virtuella förrän hello lån kopplar hello virtuell hårddisk toohello felsökning VM släpps.

1. Demontera hello datadisk på den Virtuella återställningsdatorn i RDP-session från. Du behöver hello disknumret från hello tidigare `Get-Disk` cmdlet. Använd sedan `Set-Disk` tooset hello disken som:

    ```powershell
    Set-Disk -Number 2 -IsOffline $True
    ```

    Bekräfta hello disk nu anges som offline med `Get-Disk` igen. hello visas följande exempel hello disk nu anges som offline:

    ```powershell
    Number   Friendly Name   Serial Number   HealthStatus   OperationalStatus   Total Size   Partition
                                                                                             Style
    ------   -------------   -------------   ------------   -----------------   ----------   ----------
    0        Virtual HD                                     Healthy             Online       127 GB MBR
    1        Virtual HD                                     Healthy             Online       50 GB MBR
    2        Msft Virtu...                                  Healthy             Offline      127 GB MBR
    ```

2. Avsluta RDP-session. Ta bort hello virtuella hårddisken från hello felsökning VM från Azure PowerShell-sessionen.

    ```powershell
    $myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMRecovery"
    Remove-AzureRmVMDataDisk -VM $myVM -Name "DataDisk"
    Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
    ```


## <a name="create-vm-from-original-hard-disk"></a>Skapa virtuell dator från den ursprungliga hårddisken
toocreate en virtuell dator från den ursprungliga virtuella hårddisken använder [Azure Resource Manager-mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd-existing-vnet). hello faktiska JSON-mallen finns på hello följande länk:

- https://Raw.githubusercontent.com/Azure/Azure-Quickstart-Templates/Master/201-VM-Specialized-VHD-existing-vnet/azuredeploy.JSON

hello mallen distribuerar en virtuell dator i ett befintligt virtuellt nätverk med hjälp av hello VHD URL från hello tidigare kommandot. hello följande exempel distribuerar hello mallen toohello resursgrupp med namnet `myResourceGroup`:

```powershell
New-AzureRmResourceGroupDeployment -Name myDeployment -ResourceGroupName myResourceGroup `
  -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/201-vm-specialized-vhd-existing-vnet/azuredeploy.json
```

Svaret hello efterfrågar hello mall som namn på virtuell dator, OS-typen och VM-storlek. Hej `osDiskVhdUri` är hello densamma som används när du ansluter hello befintlig virtuell hårddisk toohello felsökning VM som tidigare.


## <a name="re-enable-boot-diagnostics"></a>Återaktivera startdiagnostikinställningar

När du skapar den virtuella datorn från hello befintlig virtuell hårddisk får startdiagnostikinställningar inte automatiskt aktiveras. hello följande exempel aktiveras hello diagnostiska tillägg på hello virtuella datorn med namnet `myVMDeployed` i hello resursgrupp med namnet `myResourceGroup`:

```powershell
$myVM = Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVMDeployed"
Set-AzureRmVMBootDiagnostics -ResourceGroupName myResourceGroup -VM $myVM -enable
Update-AzureRmVM -ResourceGroup "myResourceGroup" -VM $myVM
```

## <a name="next-steps"></a>Nästa steg
Om du har problem med anslutning tooyour VM finns [felsökning av RDP-anslutningar tooan Azure VM](troubleshoot-rdp-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). Problem med att komma åt program som körs på den virtuella datorn finns [felsökning av problem med nätverksanslutningen på en Windows-VM](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

Mer information om hur du använder Resource Manager finns [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
