---
title: "aaaCreate och hantera virtuella Windows-datorer i Azure som använder flera nätverkskort | Microsoft Docs"
description: "Lär dig hur toocreate och hantera en virtuell Windows-dator som har flera nätverkskort anslutna tooit med hjälp av Azure PowerShell eller Resource Manager-mallar."
services: virtual-machines-windows
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
ms.assetid: 9bff5b6d-79ac-476b-a68f-6f8754768413
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: c3c7d7569aca6f047238146d84b2ffccf05d4079
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-a-windows-virtual-machine-that-has-multiple-nics"></a>Skapa och hantera en virtuell Windows-dator som har flera nätverkskort
Virtuella datorer (VM) i Azure kan ha flera virtuella nätverk gränssnittet kort (NIC) som är anslutna toothem. Ett vanligt scenario är toohave olika undernät för frontend och backend-anslutning eller ett nätverk dedikerad tooa övervakning eller lösning för säkerhetskopiering. Den här artikeln beskrivs hur toocreate en virtuell dator som har flera nätverkskort anslutna tooit. Du också lära dig hur tooadd eller ta bort nätverkskort från en befintlig virtuell dator. Olika [VM-storlekar](sizes.md) stöder olika antal nätverkskort, så därför storlek den virtuella datorn.

Detaljerad information, inklusive hur toocreate flera nätverkskort i en egen PowerShell-skript, se [distribuera flera NIC VMs](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md).

## <a name="prerequisites"></a>Krav
Se till att du har hello [senaste Azure PowerShell-versionen installerad och konfigurerad](/powershell/azure/overview).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn inkluderar *myResourceGroup*, *myVnet*, och *myVM*.


## <a name="create-a-vm-with-multiple-nics"></a>Skapa en virtuell dator med flera nätverkskort
Först skapa en resursgrupp. hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *EastUs* plats:

```powershell
New-AzureRmResourceGroup -Name "myResourceGroup" -Location "EastUS"
```

### <a name="create-virtual-network-and-subnets"></a>Skapa virtuella nätverk och undernät
Ett vanligt scenario är för ett virtuellt nätverk toohave två eller flera undernät. Ett undernät kan vara för frontend-trafik, hello andra för backend-trafik. tooconnect tooboth undernät kan du använda flera nätverkskort på den virtuella datorn.

1. Definiera två undernät för virtuellt nätverk med [ny AzureRmVirtualNetworkSubnetConfig](/powershell/module/azurerm.network/new-azurermvirtualnetworksubnetconfig). hello följande exempel definierar hello undernät för *mySubnetFrontEnd* och *mySubnetBackEnd*:

    ```powershell
    $mySubnetFrontEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetFrontEnd" `
        -AddressPrefix "192.168.1.0/24"
    $mySubnetBackEnd = New-AzureRmVirtualNetworkSubnetConfig -Name "mySubnetBackEnd" `
        -AddressPrefix "192.168.2.0/24"
    ```

2. Skapa virtuella nätverk och undernät med [New-AzureRmVirtualNetwork](/powershell/module/azurerm.network/new-azurermvirtualnetwork). hello följande exempel skapas ett virtuellt nätverk med namnet *myVnet*:

    ```powershell
    $myVnet = New-AzureRmVirtualNetwork -ResourceGroupName "myResourceGroup" `
        -Location "EastUs" `
        -Name "myVnet" `
        -AddressPrefix "192.168.0.0/16" `
        -Subnet $mySubnetFrontEnd,$mySubnetBackEnd
    ```


### <a name="create-multiple-nics"></a>Skapa flera nätverkskort
Skapa två nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface). Koppla en NIC toohello frontend undernät och ett nätverkskort toohello backend-undernät. hello följande exempel skapar nätverkskort med namnet *myNic1* och *myNic2*:

```powershell
$frontEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetFrontEnd'}
$myNic1 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic1" `
    -Location "EastUs" `
    -SubnetId $frontEnd.Id

$backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}
$myNic2 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
    -Name "myNic2" `
    -Location "EastUs" `
    -SubnetId $backEnd.Id
```

Vanligtvis du också skapa en [nätverkssäkerhetsgruppen](../../virtual-network/virtual-networks-nsg.md) eller [belastningsutjämnare](../../load-balancer/load-balancer-overview.md) toohelp hantera och distribuera trafik mellan dina virtuella datorer. Hej [mer detaljerad flera NIC VM](../../virtual-network/virtual-network-deploy-multinic-arm-ps.md) artikeln visar hur du skapar en nätverkssäkerhetsgrupp och tilldelar nätverkskort.

### <a name="create-hello-virtual-machine"></a>Skapa hello virtuell dator
Nu vill starta toobuild VM-konfiguration. Varje VM-storlek har en gräns för hello Totalt antal nätverkskort som du lägger till tooa VM. Mer information finns i [Windows VM-storlekar](sizes.md).

1. Ange dina autentiseringsuppgifter för VM-toohello `$cred` variabeln på följande sätt:

    ```powershell
    $cred = Get-Credential
    ```

2. Definiera den virtuella datorn med [nya AzureRmVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig). hello följande exempel definieras en virtuell dator med namnet *myVM* och använder en VM-storlek som har stöd för fler än två nätverkskort (*Standard_DS3_v2*):

    ```powershell
    $vmConfig = New-AzureRmVMConfig -VMName "myVM" -VMSize "Standard_DS3_v2"
    ```

3. Skapa hello resten av VM-konfiguration med [Set AzureRmVMOperatingSystem](/powershell/module/azurerm.compute/set-azurermvmoperatingsystem) och [Set AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage). hello följande exempel skapar en Windows Server 2016 VM:

    ```powershell
    $vmConfig = Set-AzureRmVMOperatingSystem -VM $vmConfig `
        -Windows `
        -ComputerName "myVM" `
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
    $vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig `
        -PublisherName "MicrosoftWindowsServer" `
        -Offer "WindowsServer" `
        -Skus "2016-Datacenter" `
        -Version "latest"
   ```

4. Koppla hello två nätverkskort som du skapade tidigare med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic1.Id -Primary
    $vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $myNic2.Id
    ```

5. Skapa slutligen den virtuella datorn med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm):

    ```powershell
    New-AzureRmVM -VM $vmConfig -ResourceGroupName "myResourceGroup" -Location "EastUs"
    ```

## <a name="add-a-nic-tooan-existing-vm"></a>Lägg till ett NIC-tooan befintliga VM
tooadd som en virtuell NIC-tooan befintliga VM frigöra hello VM, lägga till hello virtuella nätverkskort, och starta sedan hello VM.

1. Frigöra hello virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Hämta hello befintlig serverkonfiguration hello VM med [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). hello följande exempel hämtar information för hello virtuella datorn med namnet *myVM* i *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. hello följande exempel skapas ett virtuellt nätverkskort med [ny AzureRmNetworkInterface](/powershell/module/azurerm.network/new-azurermnetworkinterface) med namnet *myNic3* som är kopplad för*mySubnetBackEnd*. hello virtuellt nätverkskort är ansluten toohello virtuella datorn med namnet *myVM* i *myResourceGroup* med [Lägg till AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/add-azurermvmnetworkinterface):

    ```powershell
    # Get info for hello back end subnet
    $myVnet = Get-AzureRmVirtualNetwork -Name "myVnet" -ResourceGroupName "myResourceGroup"
    $backEnd = $myVnet.Subnets|?{$_.Name -eq 'mySubnetBackEnd'}

    # Create a virtual NIC
    $myNic3 = New-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" `
        -Name "myNic3" `
        -Location "EastUs" `
        -SubnetId $backEnd.Id

    # Get hello ID of hello new virtual NIC and add tooVM
    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "MyNic3").Id
    Add-AzureRmVMNetworkInterface -VM $vm -Id $nicId | Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```

    ### <a name="primary-virtual-nics"></a>Primära virtuella nätverkskort
    En av hello nätverkskort på en VM med flera nätverkskort måste toobe primära. Om en av hello befintliga virtuella nätverkskort på hello VM har redan angetts som primär, du kan hoppa över det här steget. hello följande exempel förutsätter att det finns nu två virtuella nätverkskort på en virtuell dator och du vill tooadd hello första NIC (`[0]`) som primär hello:
        
    ```powershell
    # List existing NICs on hello VM and find which one is primary
    $vm.NetworkProfile.NetworkInterfaces
    
    # Set NIC 0 toobe primary
    $vm.NetworkProfile.NetworkInterfaces[0].Primary = $true
    $vm.NetworkProfile.NetworkInterfaces[1].Primary = $false
    
    # Update hello VM state in Azure
    Update-AzureRmVM -VM $vm -ResourceGroupName "myResourceGroup"
    ```

4. Starta virtuell dator med hello [Start AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -ResourceGroupName "myResourceGroup" -Name "myVM"
    ```

## <a name="remove-a-nic-from-an-existing-vm"></a>Ta bort ett nätverkskort från en befintlig virtuell dator
tooremove ett virtuellt nätverkskort från en befintlig virtuell dator frigöra hello VM, ta bort hello virtuella nätverkskortet och sedan starta hello VM.

1. Frigöra hello virtuell dator med [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm). hello följande exempel tar bort hello virtuella datorn med namnet *myVM* i *myResourceGroup*:

    ```powershell
    Stop-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

2. Hämta hello befintlig serverkonfiguration hello VM med [Get-AzureRmVm](/powershell/module/azurerm.compute/get-azurermvm). hello följande exempel hämtar information för hello virtuella datorn med namnet *myVM* i *myResourceGroup*:

    ```powershell
    $vm = Get-AzureRmVm -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```

3. Hämta information om hello NIC ta bort med [Get-AzureRmNetworkInterface](/powershell/module/azurerm.network/get-azurermnetworkinterface). hello följande exempel hämtar information *myNic3*:

    ```powershell
    # List existing NICs on hello VM if you need toodetermine NIC name
    $vm.NetworkProfile.NetworkInterfaces

    $nicId = (Get-AzureRmNetworkInterface -ResourceGroupName "myResourceGroup" -Name "myNic3").Id   
    ```

4. Ta bort hello nätverkskortet med [ta bort AzureRmVMNetworkInterface](/powershell/module/azurerm.compute/remove-azurermvmnetworkinterface) och sedan uppdatera hello virtuell dator med [uppdatering AzureRmVm](/powershell/module/azurerm.compute/update-azurermvm). hello följande exempel tar bort *myNic3* som erhålls av `$nicId` i hello föregående steg:

    ```powershell
    Remove-AzureRmVMNetworkInterface -VM $vm -NetworkInterfaceIDs $nicId | `
        Update-AzureRmVm -ResourceGroupName "myResourceGroup"
    ```   

5. Starta virtuell dator med hello [Start AzureRmVm](/powershell/module/azurerm.compute/start-azurermvm):

    ```powershell
    Start-AzureRmVM -Name "myVM" -ResourceGroupName "myResourceGroup"
    ```   

## <a name="create-multiple-nics-with-templates"></a>Skapa flera nätverkskort med mallar
Azure Resource Manager-mallar ger ett sätt toocreate flera instanser av en resurs under distributionen, till exempel skapa flera nätverkskort. Resource Manager-mallar använda deklarativa JSON filer toodefine din miljö. Mer information finns i [översikt över Azure Resource Manager](../../azure-resource-manager/resource-group-overview.md). Du kan använda *kopiera* toospecify hello antal instanser toocreate:

```json
"copy": {
    "name": "multiplenics",
    "count": "[parameters('count')]"
}
```

Mer information finns i [skapa flera instanser med *kopiera*](../../resource-group-create-multiple.md). 

Du kan också använda `copyIndex()` tooappend ett antal tooa resursnamn. Du kan sedan skapa *myNic1*, *MyNic2* och så vidare. hello visar följande kod ett exempel på att lägga till hello indexvärde:

```json
"name": "[concat('myNic', copyIndex())]", 
```

Du kan läsa en komplett exempel på [skapa flera nätverkskort med Resource Manager-mallar](../../virtual-network/virtual-network-deploy-multinic-arm-template.md).

## <a name="next-steps"></a>Nästa steg
Granska [Windows VM-storlekar](sizes.md) när du försöker toocreate en virtuell dator som har flera nätverkskort. Betala uppmärksamhet toohello maximalt antal nätverkskort som har stöd för varje VM-storlek. 


