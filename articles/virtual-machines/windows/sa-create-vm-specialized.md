---
title: "aaaCreate virtuell dator från en särskild disk i Azure | Microsoft Docs"
description: "Skapa en ny virtuell dator genom att koppla en särskild ohanterade disk i hello Resource Manager-distributionsmodellen."
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: c88f213b6629a6c1d6ff5845e76c2f7719672714
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-from-a-specialized-vhd-in-a-storage-account"></a>Skapa en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto

Skapa en ny virtuell dator genom att koppla en särskild ohanterade disk som hello OS-disken med hjälp av Powershell. En specialiserad disk är en kopia av VHD från en befintlig virtuell dator som underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn. 

Du kan välja mellan två alternativ:
* [Överföra en virtuell hårddisk](create-vm-specialized.md#option-1-upload-a-specialized-vhd)
* [Kopiera hello VHD från en befintlig virtuell Azure-dator](create-vm-specialized.md#option-2-copy-an-existing-azure-vm)

## <a name="before-you-begin"></a>Innan du börjar
Om du använder PowerShell kan du se till att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. Kör följande kommando tooinstall hello den.

```powershell
Install-Module AzureRM.Compute 
```
Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


## <a name="option-1-upload-a-specialized-vhd"></a>Alternativ 1: Ladda upp en särskild virtuell Hårddisk

Du kan överföra hello VHD från en särskild virtuell dator som skapats med ett lokalt virtualisering verktyg, som Hyper-V eller en virtuell dator som har exporterats från ett annat moln.

### <a name="prepare-hello-vm"></a>Förbereda hello VM
Du kan ladda upp en särskild virtuell Hårddisk som har skapats med en lokal dator eller en virtuell Hårddisk som exporterats från ett annat moln. En specialiserad VHD underhåller hello användarkonton, program och andra tillståndsdata från den ursprungliga virtuella datorn. Om du avser toouse hello VHD som-är toocreate en ny virtuell dator, kontrollera hello följande steg har slutförts. 
  
  * [Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). **Inte** generalisera hello VM med hjälp av Sysprep.
  * Ta bort gäst virtualiseringsverktyg och agenter som installerats på hello VM (d.v.s. VMware-verktyg).
  * Se till att hello VM är konfigurerade toopull dess IP-adress och DNS-inställningarna via DHCP. Detta säkerställer att hello-servern hämtar en IP-adress inom hello VNet när den startas. 


### <a name="get-hello-storage-account"></a>Hämta hello storage-konto
Du behöver ett lagringskonto i Azure toostore hello upp VM-avbildning. Du kan använda ett befintligt lagringskonto eller skapa en ny. 

tooshow hello tillgängliga storage-konton, skriver du:

```powershell
Get-AzureRmStorageAccount
```

Om du vill toouse ett befintligt lagringskonto fortsätta toohello [överför hello VM-avbildning](#upload-the-vm-vhd-to-your-storage-account) avsnitt.

Följ dessa steg om du behöver toocreate ett lagringskonto:

1. Du måste hello namnet på hello resursgruppen där hello storage-konto ska skapas. toofind ut hello resursgrupper i din prenumeration, typ:
   
    ```powershell
    Get-AzureRmResourceGroup
    ```

    en resursgrupp med namnet toocreate **myResourceGroup** i hello **västra USA** region, typ:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "West US"
    ```

2. Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "West US" `
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
### <a name="upload-hello-vhd-tooyour-storage-account"></a>Överför hello VHD tooyour storage-konto
Använd hello [Lägg till AzureRmVhd](/powershell/module/azurerm.compute/add-azurermvhd) cmdlet tooupload hello avbildningen tooa behållare på ditt lagringskonto. Det här exemplet överföringar hello filen **myVHD.vhd** från `"C:\Users\Public\Documents\Virtual hard disks\"` tooa lagringskontonamnet **mittlagringskonto** i hello **myResourceGroup** resursgruppen. hello-fil placeras i hello behållare med namnet **minbehållare** och hello nytt filnamn blir **myUploadedVHD.vhd**.

```powershell
$rgName = "myResourceGroup"
$urlOfUploadedImageVhd = "https://mystorageaccount.blob.core.windows.net/mycontainer/myUploadedVHD.vhd"
Add-AzureRmVhd -ResourceGroupName $rgName -Destination $urlOfUploadedImageVhd `
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

Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete.


## <a name="option-2-copy-hello-vhd-from-an-existing-azure-vm"></a>Alternativ 2: Kopiera hello VHD från en befintlig virtuell Azure-dator

Du kan kopiera en VHD tooanother storage-konto toouse när du skapar en ny, dubbla virtuell dator.

### <a name="before-you-begin"></a>Innan du börjar
Se till att du:

* Innehåller information om hello **käll- och storage-konton**. För hello källa VM behöver du toohave hello storage-konto och en behållare namn. Vanligtvis hello behållarens namn kommer att **virtuella hårddiskar**. Du måste också toohave en mål-lagringskontot. Om du inte redan har ett kan du skapa en med hjälp av antingen hello-portal (**fler tjänster** > lagringskonton > Lägg till) eller med hjälp av hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet. 
* Har hämtat och installerat hello [AzCopy verktyget](../../storage/common/storage-use-azcopy.md). 

### <a name="deallocate-hello-vm"></a>Frigöra hello VM
Frigöra hello VM, vilket frigör hello VHD toobe kopieras. 

* **Portalen**: Klicka på **virtuella datorer** > **myVM** > Stoppa
* **PowerShell**: Använd [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) toostop (frigöra) hello virtuella datorn med namnet **myVM** i resursgruppen **myResourceGroup**.

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroup -Name myVM
```

Hej **Status** för hello VM i hello Azure portal ändras från **stoppad** för**Stoppad (frigjord)**.

### <a name="get-hello-storage-account-urls"></a>Hämta URL: er för hello storage-konto
Du måste hello URL: er för hello käll- och storage-konton. hello URL: er se ut: `https://<storageaccount>.blob.core.windows.net/<containerName>/`. Om du redan vet hello namn på kontot och en behållare kan du bara ersätta hello information mellan hello hakparenteser toocreate URL: en. 

Du kan använda hello Azure-portalen eller Azure Powershell tooget hello URL:

* **Portalen**: Klicka på hello  **>**  för **fler tjänster** > **lagringskonton**  >   *lagringskontot* > **Blobbar** och käll-VHD-filen är förmodligen i hello **virtuella hårddiskar** behållare. Klicka på **egenskaper** för hello-behållaren och kopiera hello text med etiketten **URL**. Du behöver hello URL: er för hello käll- och behållare. 
* **PowerShell**: Använd [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) tooget hello information för den virtuella datorn med namnet **myVM** i hello resursgruppen **myResourceGroup**. Hello resultat, titta i hello **lagringsprofil** avsnittet hello **Vhd-Uri**. hello första delen av hello Uri är hello URL toohello behållare och hello sista delen är hello hello VM OS VHD namn.

```powershell
Get-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
``` 

## <a name="get-hello-storage-access-keys"></a>Hämta hello åtkomstnycklar för lagring
Hitta hello snabbtangenter för hello käll- och storage-konton. Läs mer om åtkomstnycklarna [om Azure storage-konton](../../storage/common/storage-create-storage-account.md).

* **Portalen**: Klicka på **fler tjänster** > **lagringskonton** > *lagringskonto*  >  **Åtkomstnycklar**. Kopiera hello nyckel som är märkta som **key1**.
* **PowerShell**: Använd [Get-AzureRmStorageAccountKey](/powershell/module/azurerm.storage/get-azurermstorageaccountkey) tooget hello lagringsnyckel för hello lagringskontot **mittlagringskonto** i hello resursgruppen  **myResourceGroup**. Kopiera hello nyckel med namnet **key1**.

```powershell
Get-AzureRmStorageAccountKey -Name mystorageaccount -ResourceGroupName myResourceGroup
```

### <a name="copy-hello-vhd"></a>Kopiera hello VHD
Du kan kopiera filer mellan storage-konton med hjälp av AzCopy. För hello målbehållare om hello angivna behållaren inte finns, kommer den att skapas för dig. 

toouse AzCopy, öppna Kommandotolken på den lokala datorn och navigera toohello mappen där AzCopy är installerat. Det ser ut ungefär för*C:\Program Files (x86) \Microsoft SDKs\Azure\AzCopy*. 

toocopy alla hello filer i en behållare, använda hello **/S** växla. Detta kan vara används toocopy hello OS VHD och alla data hello diskar om de finns i hello samma behållare. Det här exemplet illustrerar hur toocopy alla hello filer i hello behållaren **mysourcecontainer** i lagringskonto **mysourcestorageaccount** toohello behållare **mydestinationcontainer**  i hello **mydestinationstorageaccount** storage-konto. Ersätt hello namnen på hello storage-konton och behållare med din egen. Ersätt `<sourceStorageAccountKey1>` och `<destinationStorageAccountKey1>` med egna nycklar.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
    /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
    /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> /S
```

Om du bara vill toocopy en viss VHD i en behållare med flera filer kan du också ange hello filnamn hello /Pattern växeln. I det här exemplet endast hello-fil som heter **myFileName.vhd** ska kopieras.

```
AzCopy /Source:https://mysourcestorageaccount.blob.core.windows.net/mysourcecontainer `
  /Dest:https://mydestinationatorageaccount.blob.core.windows.net/mydestinationcontainer `
  /SourceKey:<sourceStorageAccountKey1> /DestKey:<destinationStorageAccountKey1> `
  /Pattern:myFileName.vhd
```


När den är klar, visas ett meddelande som ser ut ungefär så:

```
Finished 2 of total 2 file(s).
[2016/10/07 17:37:41] Transfer summary:
-----------------
Total files transferred: 2
Transfer successfully:   2
Transfer skipped:        0
Transfer failed:         0
Elapsed time:            00.00:13:07
```

### <a name="troubleshooting"></a>Felsökning
* När du använder AZCopy, om du ser hello felet ”servern inte kunde tooauthenticate hello begäran”, kontrollera hello värdet för hello Authorization-huvud är formaterad korrekt inklusive hello signatur. Om du använder nyckel 2 eller hello sekundära lagringsplatsen nyckeln kan du prova med hello primära eller 1 lagringsutrymme nyckel.

## <a name="create-hello-new-vm"></a>Skapa hello ny virtuell dator 

Du behöver toocreate nätverk och andra toobe för VM-resurser som används av hello ny virtuell dator.

### <a name="create-hello-subnet-and-vnet"></a>Skapa hello undernät och virtuella nätverk

Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).

1. Skapa hello undernät. Det här exemplet skapar ett undernät med namnet **mySubNet**, i resursgrupp hello **myResourceGroup**, och anger hello undernätsprefixet för IP-adress för**10.0.0.0/24**.
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubNet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Skapa hello vNet. Det här exemplet anger hello virtuella nätverket namnet toobe **myVnetName**, hello plats för**västra USA**, och hello-adressprefix för hello virtuellt nätverk för**10.0.0.0/16**. 
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnetName"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-nic"></a>Skapa en offentlig IP-adress och ett nätverkskort
tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.

1. Skapa hello offentlig IP. I det här exemplet hello offentliga IP-adressnamn har angetts för**myIP**.
   
    ```powershell
    $ipName = "myIP"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Skapa hello NIC. I det här exemplet hello NIC namn har angetts för**myNicName**.
   
    ```powershell
    $nicName = "myNicName"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName `
    -Location $location -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Skapa hello nätverkssäkerhetsgruppen och en RDP-regel
toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389. Eftersom hello VHD för hello ny virtuell dator har skapats från ett befintligt specialiserat kan VM efter hello VM skapas du använda ett befintligt konto från hello virtuella källdatorn som hade behörighet toolog med RDP.
Det här exemplet anger hello NSG namn för**myNsg** och hello RDP Regelnamn för**myRdpRule**.

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389
$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
    
```

Mer information om slutpunkter och NSG-regler finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

### <a name="set-hello-vm-name-and-size"></a>Ange namn på virtuell hello och storlek

Det här exemplet anger hello namn på virtuell dator för ”myVM” och hello VM storleken för ”Standard_A2”.
```powershell
$vmName = "myVM"
$vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize "Standard_A2"
```

### <a name="add-hello-nic"></a>Lägg till hello NIC
    
```powershell
$vm = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id
```
    
    
### <a name="configure-hello-os-disk"></a>Konfigurera hello OS-disk

1. Ange hello URI för hello VHD som du laddat upp eller kopieras. I det här exemplet hello VHD-fil med namnet **myOsDisk.vhd** sparas i ett lagringskonto med namnet **Mittlagringskonto** i en behållare med namnet **Minbehållare**.

    ```powershell
    $osDiskUri = "https://myStorageAccount.blob.core.windows.net/myContainer/myOsDisk.vhd"
    ```
2. Lägga till hello OS-disk. I det här exemplet är hello termen ”osDisk” appened toohello VM namn toocreate hello OS-disknamnet när hello OS-disken skapas. Det här exemplet anger också att den här Windows-baserade virtuella Hårddisken ska vara anslutna toohello VM som hello OS-disk.
    
    ```powershell
    $osDiskName = $vmName + "osDisk"
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri -CreateOption attach -Windows
    ```

Valfritt: Om du har datadiskar som behöver toobe kopplade toohello VM, Lägg till hello datadiskar med hjälp av hello URL: er för data virtuella hårddiskar och hello lämpliga logiska enhetsnummer (Lun).

```powershell
$dataDiskName = $vmName + "dataDisk"
$vm = Add-AzureRmVMDataDisk -VM $vm -Name $dataDiskName -VhdUri $dataDiskUri -Lun 1 -CreateOption attach
```

När du använder ett lagringskonto, hello data och operativsystemet disk webbadresser ut ungefär så här: `https://StorageAccountName.blob.core.windows.net/BlobContainerName/DiskName.vhd`. Du hittar det på hello portal genom att bläddra toohello mål lagringsbehållaren, klicka på hello operativsystem eller data VHD som har kopierats och sedan kopiera hello innehållet i hello-URL.


### <a name="complete-hello-vm"></a>Slutföra hello VM 

Skapa hello VM som använder hello-konfigurationer som vi just har skapat.

```powershell
#Create hello new VM
New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
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
$vmList = Get-AzureRmVM -ResourceGroupName $rgName
$vmList.Name
```

## <a name="next-steps"></a>Nästa steg
toosign i tooyour nya virtuella datorn, bläddra toohello VM i hello [portal](https://portal.azure.com), klickar du på **Anslut**, och öppna hello Remote Desktop RDP-filen. Använd hello autentiseringsuppgifter för den ursprungliga virtuella toosign i tooyour ny virtuell dator. Mer information finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md).

