---
title: "aaaCreate en hanterad Azure-VM från en virtuell Hårddisk generaliserad lokalt | Microsoft Docs"
description: "Överför en generaliserad virtuell Hårddisk tooAzure och använda den toocreate nya virtuella datorer i hello Resource Manager-distributionsmodellen."
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
ms.date: 05/19/2017
ms.author: cynthn
ms.openlocfilehash: 2fd0c0eec922e6ca8af4e712c1bceb1f9466105c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-a-generalized-vhd-and-use-it-toocreate-new-vms-in-azure"></a>Överföra en generaliserad virtuell Hårddisk och använder den toocreate nya virtuella datorer i Azure

Det här avsnittet vägleder dig genom med hjälp av PowerShell tooupload en VHD från en generaliserad virtuell tooAzure, skapa en avbildning från hello VHD och skapa en ny virtuell dator från den avbildningen. Du kan överföra en virtuell Hårddisk som exporteras från ett verktyg för virtualisering av lokalt eller från ett annat moln. Med hjälp av [hanterade diskar](managed-disks-overview.md) för hello ny virtuell dator förenklar hello VM hantering och ger bättre tillgänglighet när hello VM placeras i en tillgänglighetsuppsättning. 

Om du vill toouse ett exempelskript finns [exempel på skript tooupload en VHD-tooAzure och skapa en ny virtuell dator](../scripts/virtual-machines-windows-powershell-upload-generalized-script.md)

## <a name="before-you-begin"></a>Innan du börjar

- Du bör följa innan du laddar upp en VHD-tooAzure [förbereda en Windows-VHD eller VHDX tooupload tooAzure](prepare-for-upload-vhd-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
- Granska [planera för migrering av hello tooManaged diskar](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks) innan du påbörjar migreringen för[hanterade diskar](managed-disks-overview.md).
- Kontrollera att du har hello senaste versionen av hello AzureRM.Compute PowerShell-modulen. Kör följande kommando tooinstall hello den.

    ```powershell
    Install-Module AzureRM.Compute -RequiredVersion 2.6.0
    ```
    Mer information finns i [Azure PowerShell versionshantering](/powershell/azure/overview).


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



## <a name="log-in-tooazure"></a>Logga in tooAzure
Om du inte redan har PowerShell version 1.4 eller senare installerat, läsa [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).

1. Öppna Azure PowerShell och logga in tooyour Azure-konto. Ett popup-fönster som öppnas för du tooenter dina Azure-autentiseringsuppgifter.
   
    ```powershell
    Login-AzureRmAccount
    ```
2. Hämta hello prenumerations-ID: N för din tillgängliga prenumerationer.
   
    ```powershell
    Get-AzureRmSubscription
    ```
3. Ange hello korrekt prenumeration med hjälp av hello prenumerations-ID. Ersätt  *<subscriptionID>*  med hello-ID för hello rätt prenumeration.
   
    ```powershell
    Select-AzureRmSubscription -SubscriptionId "<subscriptionID>"
    ```

## <a name="get-hello-storage-account"></a>Hämta hello storage-konto
Du behöver ett lagringskonto i Azure toostore hello upp VM-avbildning. Du kan använda ett befintligt lagringskonto eller skapa en ny. 

Om du kommer att använda hello VHD toocreate hanterade diskar för en virtuell dator, måste hello lagringskontoplatsen vara samma hello plats där du skapar hello VM.

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

    en resursgrupp med namnet toocreate **myResourceGroup** i hello **östra USA** region, typ:

    ```powershell
    New-AzureRmResourceGroup -Name myResourceGroup -Location "East US"
    ```

2. Skapa ett lagringskonto med namnet **mittlagringskonto** i den här resursgruppen genom att använda hello [New-AzureRmStorageAccount](/powershell/module/azurerm.storage/new-azurermstorageaccount) cmdlet:
   
    ```powershell
    New-AzureRmStorageAccount -ResourceGroupName myResourceGroup -Name mystorageaccount -Location "East US"`
        -SkuName "Standard_LRS" -Kind "Storage"
    ```
   
    Giltiga värden för - SkuName är:
   
   * **Standard_LRS** -lokalt redundant lagring. 
   * **Standard_ZRS** -zonen redundant lagring.
   * **Standard_GRS** -Geo-redundant lagring. 
   * **Standard_RAGRS** -geo-redundant lagring med läsbehörighet. 
   * **Premium_LRS** -Premium lokalt redundant lagring. 

## <a name="upload-hello-vhd-tooyour-storage-account"></a>Överför hello VHD tooyour storage-konto

Använd hello [Lägg till AzureRmVhd](https://msdn.microsoft.com/library/mt603554.aspx) cmdlet tooupload hello VHD tooa behållare på ditt lagringskonto. Det här exemplet överföringar hello filen *myVHD.vhd* från *”C:\Users\Public\Documents\Virtual hårddiskar\"*  tooa lagringskontonamnet *mittlagringskonto*i hello *myResourceGroup* resursgruppen. hello-fil placeras i hello behållare med namnet *minbehållare* och hello nytt filnamn blir *myUploadedVHD.vhd*.

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

Det här kommandot kan ta en stund beroende på nätverksanslutningen och hello storleken på VHD-filen, toocomplete

Spara hello **mål-URI** sökväg toouse senare om du ska toocreate hanterade diskar eller en ny virtuell dator med hjälp av hello överföra VHD.

### <a name="other-options-for-uploading-a-vhd"></a>Andra alternativ för att överföra en virtuell Hårddisk
 
 
Du kan också ladda upp en VHD tooyour storage-konto med en av följande hello:

- [AzCopy](http://aka.ms/downloadazcopy)
- [Azure Storage kopiera Blob API](https://msdn.microsoft.com/library/azure/dd894037.aspx)
- [Azure Storage Explorer överför Blobbar](https://azurestorageexplorer.codeplex.com/)
- [Storage Import/Export Service REST API-referens](https://msdn.microsoft.com/library/dn529096.aspx)
-   Du rekommenderas att använda Import/Export Service om överföring tid är längre än 7 dagar. Du kan använda [DataTransferSpeedCalculator](https://github.com/Azure-Samples/storage-dotnet-import-export-job-management/blob/master/DataTransferSpeedCalculator.html) tooestimate hello tiden från enhet för storlek och överföring av data. 
    Import/Export kan vara används tooa toocopy standardlagringskonto. Du behöver toocopy från standardlagring toopremium storage-konto med ett verktyg som AzCopy.


## <a name="create-a-managed-image-from-hello-uploaded-vhd"></a>Skapa en hanterad avbildning från hello överföra VHD 

Skapa en hanterad avbildning med hjälp av din generaliserad OS-VHD. Ersätt hello värden med din egen information.


1.  Ange först hello gemensamma parametrar:

    ```powershell
    $vmName = "myVM"
    $computerName = "myComputer"
    $vmSize = "Standard_DS1_v2"
    $location = "East US" 
    $imageName = "yourImageName"
    ```

4.  Skapa hello avbildning med hjälp av din generaliserad OS-VHD.

    ```powershell
    $imageConfig = New-AzureRmImageConfig -Location $location
    $imageConfig = Set-AzureRmImageOsDisk -Image $imageConfig -OsType Windows -OsState Generalized -BlobUri $urlOfUploadedImageVhd
    $image = New-AzureRmImage -ImageName $imageName -ResourceGroupName $rgName -Image $imageConfig
    ```

## <a name="create-a-virtual-network"></a>Skapa ett virtuellt nätverk
Skapa hello vNet och undernät för hello [virtuellt nätverk](../../virtual-network/virtual-networks-overview.md).

1. Skapa hello undernät. Det här exemplet skapar ett undernät med namnet *mySubnet* med hello adressprefixet *10.0.0.0/24*.  
   
    ```powershell
    $subnetName = "mySubnet"
    $singleSubnet = New-AzureRmVirtualNetworkSubnetConfig -Name $subnetName -AddressPrefix 10.0.0.0/24
    ```
2. Skapa hello virtuellt nätverk. Det här exemplet skapar ett virtuellt nätverk med namnet *myVnet* med hello adressprefixet *10.0.0.0/16*.  
   
    ```powershell
    $vnetName = "myVnet"
    $vnet = New-AzureRmVirtualNetwork -Name $vnetName -ResourceGroupName $rgName -Location $location `
        -AddressPrefix 10.0.0.0/16 -Subnet $singleSubnet
    ```    

## <a name="create-a-public-ip-address-and-network-interface"></a>Skapa en offentlig IP-adress och gränssnitt

tooenable kommunikation med hello virtuell dator i hello virtuella nätverk, behöver du en [offentliga IP-adressen](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) och ett nätverksgränssnitt.

1. Skapa en offentlig IP-adress. Det här exemplet skapas en offentlig IP-adress med namnet *myPip*. 
   
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

## <a name="create-hello-network-security-group-and-an-rdp-rule"></a>Skapa hello nätverkssäkerhetsgruppen och en RDP-regel

toobe kan toolog i tooyour VM som använder RDP måste toohave en nätverkssäkerhetsregeln (NSG) som gör att RDP-åtkomst på port 3389. 

Det här exemplet skapas en NSG som heter *myNsg* som innehåller en regel med namnet *myRdpRule* som tillåter RDP-trafik via port 3389. Mer information om NSG: er finns [öppna portar tooa VM i Azure med hjälp av PowerShell](nsg-quickstart-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

```powershell
$nsgName = "myNsg"
$ruleName = "myRdpRule"
$rdpRule = New-AzureRmNetworkSecurityRuleConfig -Name $ruleName -Description "Allow RDP" `
    -Access Allow -Protocol Tcp -Direction Inbound -Priority 110 `
    -SourceAddressPrefix Internet -SourcePortRange * `
    -DestinationAddressPrefix * -DestinationPortRange 3389

$nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $rgName -Location $location `
    -Name $nsgName -SecurityRules $rdpRule
```


## <a name="create-a-variable-for-hello-virtual-network"></a>Skapa en variabel för hello virtuellt nätverk

Skapa en variabel för hello slutförts virtuellt nätverk. 

```powershell
$vnet = Get-AzureRmVirtualNetwork -ResourceGroupName $rgName -Name $vnetName

```

## <a name="get-hello-credentials-for-hello-vm"></a>Hämta hello autentiseringsuppgifter för hello VM

hello öppnar följande cmdlet ett fönster där anger du en ny användare och lösenord toouse som hello lokalt administratörskonto för att få fjärråtkomst till hello VM. 

```powershell
$cred = Get-Credential
```

## <a name="add-hello-vm-name-and-size-toohello-vm-configuration"></a>Lägg till hello namn på virtuell dator och storlek toohello VM-konfiguration.

```powershell
$vm = New-AzureRmVMConfig -VMName $vmName -VMSize $vmSize
```

## <a name="set-hello-vm-image-as-source-image-for-hello-new-vm"></a>Ange hello VM-avbildning som källbilden för hello ny virtuell dator

Ange hello Källavbildningen med hello-ID för hello hanterade VM-avbildning.

```powershell
$vm = Set-AzureRmVMSourceImage -VM $vm -Id $image.Id
```

## <a name="set-hello-os-configuration-and-add-hello-nic"></a>Ange hello Operativsystemets konfiguration och Lägg till hello NIC.

Ange hello lagringstyp (PremiumLRS eller StandardLRS) och hello hello OS-diskens storlek. Det här exemplet anger hello kontotyp för*PremiumLRS*, hello diskstorleken för*128 GB* och diskcachelagring för*ReadWrite*.

```powershell
$vm = Set-AzureRmVMOSDisk -VM $vm -DiskSizeInGB 128 `
-CreateOption FromImage -Caching ReadWrite

$vm = Set-AzureRmVMOperatingSystem -VM $vm -Windows -ComputerName $computerName `
-Credential $cred -ProvisionVMAgent -EnableAutoUpdate

$vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
```

## <a name="create-hello-vm"></a>Skapa hello VM

Skapa ny virtuell dator med hello konfiguration lagras i hello hello **$vm** variabeln.

```powershell
New-AzureRmVM -VM $vm -ResourceGroupName $rgName -Location $location
```

## <a name="verify-that-hello-vm-was-created"></a>Kontrollera att hello VM skapades
När du är klar bör du se hello nyskapad VM i hello [Azure-portalen](https://portal.azure.com) under **Bläddra** > **virtuella datorer**, eller genom att använda följande hello PowerShell-kommandon:

```powershell
    $vmList = Get-AzureRmVM -ResourceGroupName $rgName
    $vmList.Name
```

## <a name="next-steps"></a>Nästa steg

toosign i tooyour nya virtuella datorn, bläddra toohello VM i hello [portal](https://portal.azure.com), klickar du på **Anslut**, och öppna hello Remote Desktop RDP-filen. Använd hello autentiseringsuppgifter för den ursprungliga virtuella toosign i tooyour ny virtuell dator. Mer information finns i [hur tooconnect och logga in tooan Azure virtuella datorn kör Windows](connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

