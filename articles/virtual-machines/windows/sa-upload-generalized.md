---
title: aaaUpload en generalize VHD toocreate flera virtuella datorer i Azure | Microsoft Docs
description: "Överför en generaliserad virtuell Hårddisk tooan Azure storage-konto toocreate en virtuell Windows-dator toouse med hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: cynthn
ms.openlocfilehash: aa1af2a0acf81685e62853de71afa51e819cb696
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-tooazure-toocreate-a-new-vm"></a>Överför en generaliserad virtuell Hårddisk tooAzure toocreate en ny virtuell dator

Det här avsnittet beskriver ladda upp en generaliserad ohanterade disk tooa storage-konto och sedan skapa en ny virtuell dator med hello upp disken. En generaliserad virtuell hårddiskavbildning har haft all personlig information bort med hjälp av Sysprep. 

Om du vill att toocreate en virtuell dator från en särskild virtuell Hårddisk i ett lagringskonto finns [skapa en virtuell dator från en särskild virtuell Hårddisk](sa-create-vm-specialized.md).

Det här avsnittet beskriver med lagringskonton, men vi rekommenderar att kunder flytta toousing hanteras diskarna i stället. För en fullständig genomgång av hur tooprepare, ladda upp och skapa en ny virtuell dator med hjälp av hanterade diskar, se [skapa en ny virtuell dator från en generaliserad virtuell Hårddisk har överförts tooAzure med hjälp av hanterade diskar](upload-generalized-managed.md).



## <a name="prepare-hello-vm"></a>Förbereda hello VM

En generaliserad virtuell Hårddisk har haft all personlig information bort med hjälp av Sysprep. Om du avser toouse hello VHD som en bild toocreate nya virtuella datorer från bör du:
  
  * [Förbereda en Windows-VHD tooupload tooAzure](prepare-for-upload-vhd-image.md). 
  * Generalisera hello virtuell dator med hjälp av Sysprep

### <a name="generalize-a-windows-virtual-machine-using-sysprep"></a>Generalisera Windows-dator med hjälp av Sysprep
Det här avsnittet beskrivs hur du toogeneralize din Windows-dator för användning som en bild. Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild. Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).

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
6. När Sysprep är klar stänger hello virtuell dator. 

> [!IMPORTANT]
> Starta inte om hello VM tills du är klar uppladdning hello VHD tooAzure eller skapa en avbildning från hello VM. Kör Sysprep toogeneralize om hello VM av misstag hämtar startas om den igen.
> 
> 


## <a name="upload-hello-vhd"></a>Överför hello VHD

Överför hello VHD tooan Azure storage-konto.

### <a name="log-in-tooazure"></a>Logga in tooAzure
Om du inte redan har PowerShell version 1.4 eller senare installerat, läsa [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

1. Öppna Azure PowerShell och logga in tooyour Azure-konto. Ett popup-fönster som öppnas för du tooenter dina Azure-autentiseringsuppgifter.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Hämta hello prenumerations-ID: N för din tillgängliga prenumerationer.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Ange hello korrekt prenumeration med hjälp av hello prenumerations-ID. Ersätt `<subscriptionID>` med hello-ID för hello rätt prenumeration.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

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
 
### <a name="start-hello-upload"></a>Starta överföring av hello 

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


## <a name="create-a-new-vm"></a>Skapa en ny virtuell dator 

Du kan nu använda hello överföra VHD toocreate en ny virtuell dator. 

### <a name="set-hello-uri-of-hello-vhd"></a>Ange hello hello VHD-URI

hello URI för hello VHD toouse har hello format: https://**mittlagringskonto**.blob.core.windows.net/**minbehållare**/**MyVhdName**VHD. I det här exemplet hello VHD med namnet **myVHD** är i hello lagringskonto **mittlagringskonto** i hello behållaren **minbehållare**.

```powershell
$imageURI = "https://mystorageaccount.blob.core.windows.net/mycontainer/myVhd.vhd"
```


### <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).

1. Skapa hello undernät. hello följande exempel skapar ett undernät med namnet **mySubnet** i hello resursgruppen **myResourceGroup** med hello adressprefixet **10.0.0.0/24**.  
   
    ```powershell
    $rgName = "myResourceGroup"
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Skapa hello virtuellt nätverk. hello följande exempel skapar ett virtuellt nätverk med namnet **myVnet** i hello **västra USA** plats med hello adressprefixet **10.0.0.0/16**.  
   
    ```powershell
    $location = "West US"
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

### <a name="create-a-public-ip-address-and-network-interface"></a>Skapa en offentlig IP-adress och gränssnitt
tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.

1. Skapa en offentlig IP-adress. Det här exemplet skapas en offentlig IP-adress med namnet **myPip**. 
   
    ```powershell
    $ipName = "myPip"
    $pip = New-AzureRmPublicIpAddress -Name $ipName -ResourceGroupName $rgName -Location $location `
        -AllocationMethod Dynamic
    ```       
2. Skapa hello NIC. Det här exemplet skapar ett nätverkskort med namnet **myNic**. 
   
    ```powershell
    $nicName = "myNic"
    $nic = New-AzureRmNetworkInterface -Name $nicName -ResourceGroupName $rgName -Location $location `
        -SubnetId $vnet.Subnets[0].Id -PublicIpAddressId $pip.Id
    ```

### <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Skapa hello nätverkssäkerhetsgruppen och en RDP-regel
toobe kan toolog i tooyour VM som använder RDP måste toohave en säkerhetsregel som tillåter RDP-åtkomst på port 3389. 

Det här exemplet skapas en NSG som heter **myNsg** som innehåller en regel med namnet **myRdpRule** som tillåter RDP-trafik via port 3389. Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"

$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name myRdpRule -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


### <a name="create-a-variable-for-hello-virtual-network"></a>Skapa en variabel för hello virtuellt nätverk
Skapa en variabel för hello slutförts virtuellt nätverk. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName
```

### <a name="create-hello-vm"></a>Skapa hello VM
hello visar följande PowerShell-skript hur tooset hello konfigurationer av virtuella datorer och Använd hello överförs VM-avbildning som hello källa för hello ny installation.



```powershell
# Enter a new user name and password toouse as hello local administrator account 
    # for remotely accessing hello VM.
    $cred = Get-Credential

    # Name of hello storage account where hello VHD is located. This example sets hello 
    # storage account name as "myStorageAccount"
    $storageAccName = "myStorageAccount"

    # Name of hello virtual machine. This example sets hello VM name as "myVM".
    $vmName = "myVM"

    # Size of hello virtual machine. This example creates "Standard_D2_v2" sized VM. 
    # See hello VM sizes documentation for more information: 
    # https://azure.microsoft.com/documentation/articles/virtual-machines-windows-sizes/
    $vmSize = "Standard_D2_v2"

    # Computer name for hello VM. This examples sets hello computer name as "myComputer".
    $computerName = "myComputer"

    # Name of hello disk that holds hello OS. This example sets hello 
    # OS disk name as "myOsDisk"
    $osDiskName = "myOsDisk"

    # Assign a SKU name. This example sets hello SKU name as "Standard_LRS"
    # Valid values for -SkuName are: Standard_LRS - locally redundant storage, Standard_ZRS - zone redundant
    # storage, Standard_GRS - geo redundant storage, Standard_RAGRS - read access geo redundant storage,
    # Premium_LRS - premium locally redundant storage. 
    $skuName = "Standard_LRS"

    # Get hello storage account where hello uploaded image is stored
    $storageAcc = Get-AzureRmStorageAccount -ResourceGroupName $rgName -AccountName $storageAccName

    # Set hello VM name and size
    $vmConfig = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize

    #Set hello Windows operating system configuration and add hello NIC
    $vm = Set-AzureRmVMOperatingSystem -VM $vmConfig -Windows -ComputerName $computerName `
        -Credential $cred -ProvisionVMAgent -EnableAutoUpdate
    $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id

    # Create hello OS disk URI
    $osDiskUri = '{0}vhds/{1}-{2}.vhd' `
        -f $storageAcc.PrimaryEndpoints.Blob.ToString(), $vmName.ToLower(), $osDiskName

    # Configure hello OS disk toobe created from hello existing VHD image (-CreateOption fromImage).
    $vm = Set-AzureRmVMOSDisk -VM $vm -Name $osDiskName -VhdUri $osDiskUri `
        -CreateOption fromImage -SourceImageUri $imageURI -Windows

    # Create hello new VM
    New-AzureRmVM -ResourceGroupName $rgName -Location $location -VM $vm
```

## <a name="verify-that-hello-vm-was-created"></a>Kontrollera att hello VM skapades
När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nästa steg
din nya virtuella datorn med Azure PowerShell finns i toomanage [hantera virtuella datorer med Azure Resource Manager och PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


