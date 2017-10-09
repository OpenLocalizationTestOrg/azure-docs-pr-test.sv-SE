---
title: "aaaCreate en virtuell Windows-dator från en särskild virtuell Hårddisk i Azure | Microsoft Docs"
description: "Skapa en ny Windows virtuell dator genom att koppla en särskild hanterade diskar som hello OS-disken med i hello Resource Manager-distributionsmodellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 3b7d3cd5-e3d7-4041-a2a7-0290447458ea
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: cynthn
ms.openlocfilehash: c72c6f4fb650e2664e87cf38ec9be62f0b2209fb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-windows-vm-from-a-specialized-disk"></a>Skapa en virtuell Windows-dator från en särskild disk

Skapa en ny virtuell dator genom att koppla en särskild hanterade diskar som hello OS-disken med hjälp av Powershell. En specialiserad disk är en kopia av virtuell hårddisk (VHD) från en befintlig virtuell dator som underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn. 

När du använder en särskild VHD toocreate en ny virtuell dator hello hello ny virtuell dator behåller hello datornamnet för ursprungliga virtuella datorn. Andra datorspecifik information sparas också och i vissa fall kan den här dubblettinformation kan orsaka problem. Ta reda på vilka typer av information om datorn dina program förlitar sig på när du kopierar en virtuell dator.

Du kan välja mellan två alternativ:
* [Överföra en virtuell hårddisk](#option-1-upload-a-specialized-vhd)
* [Kopiera en befintlig virtuell Azure-dator](#option-2-copy-an-existing-azure-vm)

Det här avsnittet visar hur toouse hanterade diskar. Om du har en äldre distribution som kräver att du använder ett lagringskonto, se [skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto](sa-create-vm-specialized.md)

## <a name="before-you-begin"></a>Innan du börjar
Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. 

```powershell
Install-Module AzureRM.Compute -RequiredVersion 2.6.0
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Alternativ 1: Ladda upp en särskild virtuell Hårddisk

Du kan överföra hello VHD från en särskild virtuell dator som skapats med ett lokalt virtualisering verktyg, som Hyper-V eller en virtuell dator som har exporterats från ett annat moln.

### <a name="prepare-hello-vm"></a>Förbereda hello VM
Om du avser toouse hello VHD som-är toocreate en ny virtuell dator, kontrollera hello följande steg har slutförts. 
  
  * [Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Inte** generalisera hello VM med hjälp av Sysprep.
  * Ta bort gäst virtualiseringsverktyg och agenter som installerats på hello VM (till exempel VMware-verktyg).
  * Se till att hello VM är konfigurerade toopull dess IP-adress och DNS-inställningarna via DHCP. Detta säkerställer att hello-servern hämtar en IP-adress inom hello VNet när den startas. 


### <a name="get-hello-storage-account"></a>Hämta hello storage-konto
Du behöver ett konto i Azure toostore hello överföra VHD. Du kan använda ett befintligt lagringskonto eller skapa en ny. 

tooshow hello tillgängliga storage-konton, skriver du:

```powershell
Get-AzureRmStorageAccount
```

Om du vill toouse ett befintligt lagringskonto fortsätta toohello [överför hello VHD](#upload-the-vhd-to-your-storage-account) avsnitt.

Följ dessa steg om du behöver toocreate ett lagringskonto:

1. Du måste hello namnet på hello resursgruppen där hello storage-konto ska skapas. toofind ut hello resursgrupper i din prenumeration, typ:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    en resursgrupp med namnet toocreate *myResourceGroup* i hello *västra USA* region, typ:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Skapa ett lagringskonto med namnet *mittlagringskonto* i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```

### <a name="upload-hello-vhd-tooyour-storage-account"></a>Överför hello VHD tooyour storage-konto 
Använd hello [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello VHD tooa behållare på ditt lagringskonto. Det här exemplet överföringar hello filen *myVHD.vhd* från `"C:\Users\Public\Documents\Virtual hard disks\"` tooa lagringskontonamnet *mittlagringskonto* i hello *myResourceGroup* resursgruppen. hello lagras i hello behållare med namnet *minbehållare* och hello nytt filnamn blir *myUploadedVHD.vhd*.

```powershell
$resourceGroupName = "myResourceGroup"
$urlOfUploadedVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $resourceGroupName -Destination $urlOfUploadedVhd `
    -LocalFilePath "C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd"
```


Om det lyckas, får du ett svar som ser ut ungefär toothis:

```powershell
MD5 hash is being calculated for hello file C:\Users\Public\Documents\Virtual hard disks\myVHD.vhd.
MD5 hash calculation is completed.
Elapsed time for hello operation: 00:03:35
Creating new page blob of size 53687091712...
Elapsed time for upload: 01:12:49

LocalFilePath           DestinationUri
-------------           --------------
C:\Users\Public\Doc...  https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd
```

Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete

### <a name="create-a-managed-disk-from-hello-vhd"></a>Skapa en hanterad disk från hello VHD

Skapa en hanterad disk från hello specialiserad VHD på din storage-konto med [ny AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Det här exemplet används **myOSDisk1** för hello disknamnet placeringar hello disk i *StandardLRS* lagring och använder *https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd* som hello URI för hello källan VHD.

Skapa en ny resursgrupp för hello ny virtuell dator.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Skapa hello nya OS-disken från hello överföra VHD. 

```powershell
$sourceUri = https://storageaccount.blob.core.windows.net/vhdcontainer/osdisk.vhd)
$osDiskName = 'myOsDisk'
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig -AccountType StandardLRS  -Location $location -CreateOption Import `
    -SourceUri $sourceUri) `
    -ResourceGroupName $destinationResourceGroup
```

## <a name="option-2-copy-an-existing-azure-vm"></a>Alternativ 2: Kopiera en befintlig virtuell Azure-dator

Du kan skapa en kopia av en virtuell dator som använder hanterade diskar genom att ta en ögonblicksbild av hello VM och sedan med hjälp av den ögonblicksbild toocreate en ny hanterade disken och en ny virtuell dator.


### <a name="take-a-snapshot-of-hello-os-disk"></a>Ta en ögonblicksbild av hello OS-disk

Du kan ta en ögonblicksbild av och en hel virtuell dator (inklusive alla diskar) eller bara en enda disk. hello följande steg visar hur tootake en ögonblicksbild av bara hello OS-disk för virtuell dator med hjälp av hello [ny AzureRmSnapshot](/powershell/module/azurerm.compute/new-azurermsnapshot) cmdlet. 

Ange vissa parametrar. 

 ```powershell
$resourceGroupName = 'myResourceGroup' 
$vmName = 'myVM'
$location = 'westus' 
$snapshotName = 'mySnapshot'  
```

Hämta hello Virtuella datorobjektet.

```powershell
$vm = Get-AzureRmVM -Name $vmName -ResourceGroupName $resourceGroupName
```
Hämta hello OS-disknamnet.

 ```powershell
$disk = Get-AzureRmDisk -ResourceGroupName $resourceGroupName -DiskName $vm.StorageProfile.OsDisk.Name
```

Skapa hello ögonblicksbild konfigurationen. 

 ```powershell
$snapshotConfig =  New-AzureRmSnapshotConfig -SourceUri $disk.Id -OsType Windows -CreateOption Copy -Location $location 
```

Ta hello ögonblicksbild.

```powershell
$snapShot = New-AzureRmSnapshot -Snapshot $snapshotConfig -SnapshotName $snapshotName -ResourceGroupName $resourceGroupName
```


Om du planerar toouse hello ögonblicksbild toocreate en virtuell dator som behöver toobe högpresterande använder hello parametern `-AccountType Premium_LRS` med hello ny AzureRmSnapshot kommando. hello-parametern skapar hello ögonblicksbild så att den lagras som en Premium hanteras Disk. Premium-hanterade diskar är dyrare än Standard. Vara säker på att du verkligen behöver Premium innan du använder hello-parametern.

### <a name="create-a-new-disk-from-hello-snapshot"></a>Skapa en ny disk från hello ögonblicksbild

Skapa en hanterad disk från hello med verktyget [ny AzureRMDisk](/powershell/module/azurerm.compute/new-azurermdisk). Det här exemplet används *myOSDisk* för hello diskens namn.

Skapa en ny resursgrupp för hello ny virtuell dator.

```powershell
$destinationResourceGroup = 'myDestinationResourceGroup'
New-AzureRmResourceGroup -Location $location -Name $destinationResourceGroup
```

Ange hello OS-disknamnet. 

```powershell
$osDiskName = 'myOsDisk'
```

Skapa hello hanterade diskar.

```powershell
$osDisk = New-AzureRmDisk -DiskName $osDiskName -Disk `
    (New-AzureRmDiskConfig  -Location $location -CreateOption Copy `
    -SourceResourceId $snapshot.Id) `
    -ResourceGroupName $destinationResourceGroup
```


## <a name="create-hello-new-vm"></a>Skapa hello ny virtuell dator 

Skapa nätverk och andra toobe för VM-resurser som används av hello ny virtuell dator.

### <a name="create-hello-subnet-and-vnet"></a>Skapa hello undernät och virtuella nätverk

Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).

Skapa hello undernät. Det här exemplet skapar ett undernät med namnet **mySubNet**, i resursgrupp hello **myDestinationResourceGroup**, och anger hello undernätsprefixet för IP-adress för**10.0.0.0/24**.
   
```powershell
$subnetName = 'mySubNet'
$singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
```

Skapa hello vNet. Det här exemplet anger hello virtuella nätverket namnet toobe **myVnetName**, hello plats för**västra USA**, och hello-adressprefix för hello virtuellt nätverk för**10.0.0.0/16**. 
   
```powershell
$vnetName = "myVnetName"
$vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $destinationResourceGroup -Location $location `
    -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
```    


### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Skapa hello nätverkssäkerhetsgruppen och en RDP-regel
toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389. Eftersom hello VHD för hello ny virtuell dator har skapats från en befintlig särskild virtuell dator kan du använda ett konto från hello virtuella källdatorn för RDP.

Det här exemplet anger hello NSG namn för**myNsg** och hello RDP Regelnamn för**myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $destinationResourceGroup -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Mer information om slutpunkter och NSG-regler finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="create-a-public-ip-address-and-nic"></a>Skapa en offentlig IP-adress och ett nätverkskort
tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.

Skapa hello offentlig IP. I det här exemplet hello offentliga IP-adressnamn har angetts för**myIP**.
   
```powershell
$ipName = "myIP"
$pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $destinationResourceGroup -Location $location `
   -AllocationMethod Dynamic
```       

Skapa hello NIC. I det här exemplet hello NIC namn har angetts för**myNicName**.
   
```powershell
$nicName = "myNicName"
$nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $destinationResourceGroup `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id -NetworkSecurityGroupId $nsg.Id
```



### <a name="set-hello-vm-name-and-size"></a>Ange namn på virtuell hello och storlek

Det här exemplet anger hello namn på virtuell dator för*myVM* och hello VM-storlek för*Standard_A2*.

```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a>Lägg till hello NIC
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    

### <a name="add-hello-os-disk"></a>Lägg till hello OS-disk 

Lägg till hello OS disk toohello konfiguration av [Set AzureRmVMOSDisk](/powershell/module/azurerm.compute/set-azurermvmosdisk). Det här exemplet anger hello hello diskens storlek för*128 GB* och bifogar hello hanterade diskar som en *Windows* OS-disken.
 
```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -ManagedDiskId $osDisk.Id -StorageAccountType StandardLRS `
    -DiskSizeInGB 128 -CreateOption Attach -Windows
```

### <a name="complete-hello-vm"></a>Slutföra hello VM 

Skapa hello VM med hjälp av [ny AzureRMVM](/powershell/module/azurerm.compute/new-azurermvm)hello konfigurationer som vi just skapade.

```powershell
New-AzureRmVM -ResourceGroupName $destinationResourceGroup -Location $location -VM $vm
```

Om det här kommandot lyckades visas utdata som liknar detta:

```powershell
RequestId IsSuccessStatusCode StatusCode ReasonPhrase
--------- ------------------- ---------- ------------
                         True         OK OK   

```

### <a name="verify-that-hello-vm-was-created"></a>Kontrollera att hello VM skapades
Du bör se hello nyskapad VM antingen i hello [Azure-portalen](https://portal.azure.com)under **Bläddra** > **virtuella datorer**, eller genom att använda följande PowerShell hello kommandon:

```powershell
$vmList = Get-AzureRmVM -ResourceGroupName $destinationResourceGroup
$vmList.Name
```

## <a name="next-steps"></a>Nästa steg
toosign i tooyour nya virtuella datorn, bläddra toohello VM i hello [portal](https://portal.azure.com), klickar du på **Anslut**, och öppna hello Remote Desktop RDP-filen. Använd hello autentiseringsuppgifter för den ursprungliga virtuella toosign i tooyour ny virtuell dator. Mer information finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md).

