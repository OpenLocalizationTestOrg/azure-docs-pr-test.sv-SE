---
title: aaaCreate en ohanterad avbildning av en generaliserad virtuell dator i Azure | Microsoft Docs
description: Skapa en unmanged avbildning av en generaliserad virtuell Windows-dator toouse toocreate flera kopior av en virtuell dator i Azure.
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
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: b8a044d20313efa6dd9b9757e61154f922134476
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-an-unmanaged-vm-image-from-an-azure-vm"></a>Hur toocreate en ohanterad VM bild av en Azure VM

Den här artikeln täcker storage-konton. Vi rekommenderar att du använder hanterade diskar och hanterade bilder i stället för ett lagringskonto. Mer information finns i [samla in en hanterad avbildning av en generaliserad virtuell dator i Azure](capture-image-resource.md).

Den här artikeln beskrivs hur du toouse Azure PowerShell toocreate en avbildning av en generaliserad virtuell Azure-dator med hjälp av ett lagringskonto. Du kan sedan använda hello avbildningen toocreate en annan virtuell dator. hello bilden innehåller hello OS-disken och hello datadiskar som är bifogade toohello virtuella datorn. hello bilden innehåller hello virtuella nätverksresurser, så du måste tooset dessa resurser när du skapar hello ny virtuell dator. 

## <a name="prerequisites"></a>Krav
Du behöver toohave Azure PowerShell version 1.0.x eller nyare. Om du inte redan har installerat PowerShell, läsa [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) för installationssteg.

## <a name="generalize-hello-vm"></a>Generalisera hello VM 
Det här avsnittet beskrivs hur du toogeneralize din Windows-dator för användning som en bild. Att generalisera en virtuell dator tar du bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild. Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).

Kontrollera att hello serverroller som körs på datorn hello stöds av Sysprep. Mer information finns i [Sysprep-stöd för serverroller](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles)

> [!IMPORTANT]
> Om du överför din VHD tooAzure för hello första gången, kontrollera att du har [förberett din virtuella dator](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) innan du kör Sysprep. 
> 
> 

Du kan också generalisera en Linux VM som använder `sudo waagent -deprovision+user` och sedan använda PowerShell toocapture hello VM. Information om hur du använder hello CLI toocapture en virtuell dator finns [hur toogeneralize och avbilda en Linux virtuella datorer med hjälp av hello Azure CLI ](../linux/capture-image.md).


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

## <a name="log-in-tooazure-powershell"></a>Logga in tooAzure PowerShell
1. Öppna Azure PowerShell och logga in tooyour Azure-konto.
   
    ```powershell
    Login-AzureRmAccount
    ```
   
    Ett popup-fönster som öppnas för du tooenter dina Azure-autentiseringsuppgifter.
2. Hämta hello prenumerations-ID: N för din tillgängliga prenumerationer.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Ange hello korrekt prenumeration med hjälp av hello prenumerations-ID.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="deallocate-hello-vm-and-set-hello-state-toogeneralized"></a>Frigöra hello VM och ange hello tillstånd toogeneralized
1. Frigöra hello VM resurser.
   
    ```powershell
    Stop-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName>
    ```
   
    Hej *Status* för hello VM i hello Azure portal ändras från **stoppad** för**Stoppad (frigjord)**.
2. Ange hello hello virtuella datorns status för för**generaliserad**. 
   
    ```powershell
    Set-AzureRmVm -ResourceGroupName <resourceGroup> -Name <vmName> -Generalized
    ```
3. Kontrollera hello status hello VM. Hej **OSState/generaliserad** avsnittet för hello VM bör ha hello **DisplayStatus** ställa in också**VM generaliserad**.  
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroup> -Name <vmName> -Status
    $vm.Statuses
    ```

## <a name="create-hello-image"></a>Skapa hello avbildning

Skapa en avbildning av ohanterade virtuell dator i hello lagring målbehållare med det här kommandot. hello avbildningen skapas i hello samma lagringskonto som hello ursprungliga virtuella datorn. Hej `-Path` parametern sparar en kopia av hello JSON-mall för hello VM tooyour lokala källdator. Hej `-DestinationContainerName` parametern är hello namnet på hello-behållare som du vill toohold bilderna. Om hello-behållaren inte finns, skapas den åt dig.
   
```powershell
Save-AzureRmVMImage -ResourceGroupName <resourceGroupName> -Name <vmName> `
    -DestinationContainerName <destinationContainerName> -VHDNamePrefix <templateNamePrefix> `
    -Path <C:\local\Filepath\Filename.json>
```
   
Du kan hämta hello URL för bilden från hello JSON mall. Gå toohello **resurser** > **storageProfile** > **osDisk** > **bild**  >  **uri** avsnitt för hello fullständiga sökväg i avbildningen. hello-URL för hello bild ser ut: `https://<storageAccountName>.blob.core.windows.net/system/Microsoft.Compute/Images/<imagesContainer>/<templatePrefix-osDisk>.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`.
   
Du kan också kontrollera hello URI i hello-portalen. hello bilden är kopierade tooa behållare med namnet **system** i ditt lagringskonto. 

## <a name="create-a-vm-from-hello-image"></a>Skapa en virtuell dator från hello avbildning

Nu kan du skapa en eller flera virtuella datorer från ohanterade hello-avbildning.

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
hello följande PowerShell Slutför hello konfigurationer av virtuella datorer och använder ohanterade avbildningen som hello källa för hello ny installation.

</br>

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

### <a name="verify-that-hello-vm-was-created"></a>Kontrollera att hello VM skapades
När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nästa steg
din nya virtuella datorn med Azure PowerShell finns i toomanage [hantera virtuella datorer med Azure Resource Manager och PowerShell](tutorial-manage-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).


