---
title: aaaCreate en hanterad avbildning i Azure | Microsoft Docs
description: "Skapa en hanterad avbildning av en generaliserad virtuell dator eller virtuell Hårddisk i Azure. Avbildningar kan använda toocreate flera virtuella datorer som använder hanterade diskar."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: cynthn
ms.openlocfilehash: d8cd6c2ce8c5d704de2c845abced85139944d682
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-image-of-a-generalized-vm-in-azure"></a>Skapa en hanterad avbildning av en generaliserad virtuell dator i Azure

En hanterad avbildning resurs kan skapas från en generaliserad virtuell dator som lagras som en hanterad disk eller en ohanterad disk i ett lagringskonto. Hej avbildningen kan sedan att använda toocreate flera virtuella datorer. 


## <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalisera hello virtuell Windows-dator med hjälp av Sysprep

Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild. Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).

Kontrollera att hello serverroller som körs på datorn hello stöds av Sysprep. Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Om du kör Sysprep innan du laddar upp din VHD tooAzure för hello första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep. 
> 
> 

1. Logga in toohello Windows-dator.
2. Öppna hello Kommandotolken som administratör. Ändra hello katalogen för**%windir%\system32\sysprep**, och kör sedan `sysprep.exe`.
3. I hello **systemförberedelseverktyget** dialogrutan **ange System Out of Box Experience (OOBE)**, och se till att hello **Generalize** är markerad.
4. I **avstängningsalternativ**väljer **avstängning**.
5. Klicka på **OK**.
   
    ![Starta Sysprep](./media/upload-generalized-managed/sysprepgeneral.png)
6. När Sysprep är klar stänger hello virtuell dator. Starta inte om hello VM.


## <a name="create-a-managed-image-in-hello-portal"></a>Skapa en hanterad avbildning i hello-portalen 

1. Öppna hello [portal](https://portal.azure.com).
2. Klicka på hello plustecknet toocreate en ny resurs.
3. Skriv i hello filter Sök **bild**.
4. Välj **bild** hello resultaten.
5. I hello **bild** bladet, klickar du på **skapa**.
6. I **namn**, Skriv ett namn för hello avbildningen.
7. Om du har mer än en prenumeration väljer hello rätt en från hello **prenumeration** listrutan.
7. I **resursgruppen** Välj antingen **Skapa nytt** och ange ett namn eller välj **från befintliga** och välj en resurs grupp toouse hello nedrullningsbara listan.
8. I **plats**, Välj hello plats för resursgruppen.
9. I **OS-typen** Välj hello typ av operativsystem, Windows eller Linux.
11. I **lagringsblob**, klickar du på **Bläddra** toolook för hello VHD i Azure-lagring.
12. I **kontotyp** Välj Standard_LRS eller Premium_LRS. Standard använder hårddiskar och Premium använder SSD-enheter. Båda använder lokalt redundant lagring.
13. I **cachelagring på Disk** Välj hello lämplig disk alternativet för cachelagring. hello alternativ är **ingen**, **skrivskyddad** och **läsningen/skrivningen**.
14. Valfritt: Du kan också lägga till en befintlig data toohello diskavbildning genom att klicka på **+ Lägg till datadisk**.  
15. När du är klar med dina val klickar du på **skapa**.
16. När hello avbildningen har skapats visas den som en **bild** resurs i hello listan över resurser i hello resursgrupp som du har valt.



## <a name="create-a-managed-image-of-a-vm-using-powershell"></a>Skapa en hanterad avbildning av en virtuell dator med hjälp av Powershell

Skapa en avbildning direkt från hello VM garanterar hello avbildningen innehåller alla hello diskar som är associerade med hello VM, inklusive hello OS-disken och eventuella hårddiskar.


Innan du börjar bör du kontrollera att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. Kör följande kommando tooinstall hello den.

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


1. Skapa några variabler.

    ```powershell
    $vmName = "myVM"
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $imageName = "myImage"
    ```
2. Se till att hello VM har frigjorts.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Ange hello hello virtuella datorns status för för**generaliserad**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized
    ```
    
4. Hämta hello virtuell dator. 

    ```powershell
    $vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $rgName
    ```

5. Skapa hello image-konfigurationen.

    ```powershell
    $image = New-AzureRmImageConfig -Location $location -SourceVirtualMachineId $vm.ID 
    ```
6. Skapa hello avbildning.

    ```powershell
    New-AzureRmImage -Image $image -ImageName $imageName -ResourceGroupName $rgName
    ``` 



## <a name="create-a-managed-image-of-a-vhd-in-powershell"></a>Skapa en hanterad avbildning av en virtuell Hårddisk i PowerShell

Skapa en hanterad avbildning med hjälp av din generaliserad OS-VHD.


1.  Ange först hello gemensamma parametrar:

    ```powershell
    $rgName = "myResourceGroupName"
    $vmName = "myVM"
    $location = "West Central US" 
    $imageName = "yourImageName"
    $osVhdUri = "https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd"
    ```
2. Step\deallocate hello VM.

    ```powershell
    Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
    ```
    
3. Markera hello VM som generaliserad.

    ```powershell
    Set-AzureRmVm -ResourceGroupName $rgName -Name $vmName -Generalized 
    ```
4.  Skapa hello avbildning med hjälp av din generaliserad OS-VHD.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $osVhdUri
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```


## <a name="create-a-managed-image-from-a-snapshot-using-powershell"></a>Skapa en hanterad avbildning från en ögonblicksbild med hjälp av Powershell

Du kan också skapa en hanterad avbildning från en ögonblicksbild av hello VHD från en generaliserad virtuell dator.

    
1. Skapa några variabler. 

    ```powershell
    $rgName = "myResourceGroup"
    $location = "EastUS"
    $snapshotName = "mySnapshot"
    $imageName = "myImage"
    ```

2. Hämta hello ögonblicksbild.

   ```powershell
   $snapshot = Get-AzureRmSnapshot -ResourceGroupName $rgName -SnapshotName $snapshotName
   ```
   
3. Skapa hello image-konfigurationen.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsState Generalized -OsType Windows -SnapshotId $snapshot.Id
    ```
4. Skapa hello avbildning.

    ```powershell
    New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ``` 
    

## <a name="next-steps"></a>Nästa steg
- Nu kan du [skapa en virtuell dator från hello generaliserad hanterad avbildning](create-vm-generalized-managed.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).    

