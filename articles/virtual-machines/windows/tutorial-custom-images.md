---
title: aaaCreate anpassade VM-avbildningar med hello Azure PowerShell | Microsoft Docs
description: "Självstudiekurs – skapa en anpassad VM-avbildning med hjälp av hello Azure PowerShell."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 3a759fe1b7e7b72f531399b0f4a99e341713c6a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-custom-image-of-an-azure-vm-using-powershell"></a>Skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av PowerShell

Anpassade avbildningar liknar marketplace-bilder, men du skapa dem själv. Anpassade avbildningar kan vara används toobootstrap konfigurationer, till exempel förinstalleras program, Programinställningar och andra OS-konfigurationer. I den här självstudiekursen skapar du en egen anpassad avbildning av en virtuell Azure-dator. Lär dig att:

> [!div class="checklist"]
> * Sysprep och generalisera virtuella datorer
> * Skapa en egen avbildning
> * Skapa en virtuell dator från en anpassad avbildning
> * Visa en lista med alla hello bilder i din prenumeration
> * Ta bort en bild

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

## <a name="before-you-begin"></a>Innan du börjar

hello stegen nedan i detalj hur tootake en befintlig virtuell dator och aktivera den till en återanvändbara anpassad avbildning som du kan använda toocreate nya VM-instanser.

toocomplete hello exempel i den här självstudiekursen, måste du ha en befintlig virtuell dator. Om det behövs, detta [skriptexempel](../scripts/virtual-machines-windows-powershell-sample-create-vm.md) kan skapa en åt dig. När gå igenom självstudiekursen hello ersätter namn hello resursgrupp och VM där det behövs.

## <a name="prepare-vm"></a>Förbereda VM

toocreate en avbildning av en virtuell dator måste tooprepare hello VM genom att generalisera hello VM, det har frigjorts, och sedan göra hello källa VM som generaliserad i Azure.

### <a name="generalize-hello-windows-vm-using-sysprep"></a>Generalisera hello virtuell Windows-dator med hjälp av Sysprep

Sysprep tar bort alla dina personlig information, bland annat och förbereder hello datorn toobe används som en bild. Mer information om Sysprep finns [hur tooUse Sysprep: en introduktion](http://technet.microsoft.com/library/bb457073.aspx).


1. Ansluta toohello virtuella datorn.
2. Öppna hello Kommandotolken som administratör. Ändra hello katalogen för*%windir%\system32\sysprep*, och kör sedan *sysprep.exe*.
3. I hello **systemförberedelseverktyget** dialogrutan *ange System Out of Box Experience (OOBE)*, och se till att hello *Generalize* är markerad.
4. I **avstängningsalternativ**väljer *avstängning* och klicka sedan på **OK**.
5. När Sysprep är klar stänger hello virtuell dator. **Starta inte om hello VM**.

### <a name="deallocate-and-mark-hello-vm-as-generalized"></a>Frigöra och markera hello VM som generaliserad

toocreate en avbildning måste hello VM toobe frigjorts och har markerats som generaliserad i Azure.

Frigjord hello VM med hjälp av [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm).

```powershell
Stop-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Force
```

Ange hello hello virtuella datorns status för för`-Generalized` med [Set AzureRmVm](/powershell/module/azurerm.compute/set-azurermvm). 
   
```powershell
Set-AzureRmVM -ResourceGroupName myResourceGroupImages -Name myVM -Generalized
```


## <a name="create-hello-image"></a>Skapa hello avbildning

Nu kan du skapa en avbildning av hello VM med hjälp av [ny AzureRmImageConfig](/powershell/module/azurerm.compute/new-azurermimageconfig) och [ny AzureRmImage](/powershell/module/azurerm.compute/new-azurermimage). hello följande exempel skapas en bild med namnet *myImage* från en virtuell dator med namnet *myVM*.

Hämta hello virtuell dator. 

```powershell
$vm = Get-AzureRmVM -Name myVM -ResourceGroupName myResourceGroupImages
```

Skapa hello image-konfigurationen.

```powershell
$image = New-AzureRmImageConfig -Location EastUS -SourceVirtualMachineId $vm.ID 
```

Skapa hello avbildning.

```powershell
New-AzureRmImage -Image $image -ImageName myImage -ResourceGroupName myResourceGroupImages
``` 

 
## <a name="create-vms-from-hello-image"></a>Skapa virtuella datorer från hello-bild

Nu när du har skapat en avbildning kan du kan skapa en eller flera nya virtuella datorer från hello avbildning. Skapa en virtuell dator från en anpassad avbildning är mycket lik toocreating en virtuell dator med hjälp av en Marketplace-avbildning. När du använder en Marketplace-avbildning har tooinformation om hello-avbildning, image-providern, erbjudande, SKU och version. Med en anpassad avbildning måste du bara tooprovide hello-ID för hello egen bildresurs. 

I följande skript hello, skapar vi en variabel *$image* toostore information om hello anpassad avbildning med hjälp av [Get-AzureRmImage](/powershell/module/azurerm.compute/get-azurermimage) och sedan använder vi [Set-AzureRmVMSourceImage](/powershell/module/azurerm.compute/set-azurermvmsourceimage)och ange hello-ID med hjälp av hello *$image* variabeln vi just skapade. 

hello skriptet skapar en virtuell dator med namnet *myVMfromImage* från våra anpassad avbildning i en ny resursgrupp med namnet *myResourceGroupFromImage* i hello *västra USA* plats.


```powershell
$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

New-AzureRmResourceGroup -Name myResourceGroupFromImage -Location EastUS

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24

$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

$pip = New-AzureRmPublicIpAddress `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name "mypublicdns$(Get-Random)" `
    -AllocationMethod Static `
    -IdleTimeoutInMinutes 4

  $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
    -Name myNetworkSecurityGroupRuleRDP `
    -Protocol Tcp `
    -Direction Inbound `
    -Priority 1000 `
    -SourceAddressPrefix * `
    -SourcePortRange * `
    -DestinationAddressPrefix * `
    -DestinationPortRange 3389 `
    -Access Allow

  $nsg = New-AzureRmNetworkSecurityGroup `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -Name myNetworkSecurityGroup `
    -SecurityRules $nsgRuleRDP

$nic = New-AzureRmNetworkInterface `
    -Name myNic `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -SubnetId $vnet.Subnets[0].Id `
    -PublicIpAddressId $pip.Id `
    -NetworkSecurityGroupId $nsg.Id

$vmConfig = New-AzureRmVMConfig `
    -VMName myVMfromImage `
    -VMSize Standard_D1 | Set-AzureRmVMOperatingSystem -Windows `
        -ComputerName myComputer `
        -Credential $cred 

# Here is where we create a variable toostore information about hello image 
$image = Get-AzureRmImage `
    -ImageName myImage `
    -ResourceGroupName myResourceGroupImages

# Here is where we specify that we want toocreate hello VM from and image and provide hello image ID
$vmConfig = Set-AzureRmVMSourceImage -VM $vmConfig -Id $image.Id

$vmConfig = Add-AzureRmVMNetworkInterface -VM $vmConfig -Id $nic.Id

New-AzureRmVM `
    -ResourceGroupName myResourceGroupFromImage `
    -Location EastUS `
    -VM $vmConfig
```

## <a name="image-management"></a>Avbildningshantering 

Här följer några exempel på vanliga hanteringsuppgifter för avbildningen och hur toocomplete dem med hjälp av PowerShell.

Visa alla avbildningar efter namn.

```powershell
$images = Find-AzureRMResource -ResourceType Microsoft.Compute/images 
$images.name
```

Ta bort en bild. Det här exemplet tar bort hello bild med namnet *myOldImage* från hello *myResourceGroup*.

```powershell
Remove-AzureRmImage `
    -ImageName myOldImage `
    -ResourceGroupName myResourceGroup
```

## <a name="next-steps"></a>Nästa steg

I kursen får skapat du en anpassad VM-avbildning. Du har lärt dig att:

> [!div class="checklist"]
> * Sysprep och generalisera virtuella datorer
> * Skapa en egen avbildning
> * Skapa en virtuell dator från en anpassad avbildning
> * Visa en lista med alla hello bilder i din prenumeration
> * Ta bort en bild

Avancera toohello nästa självstudiekurs toolearn om hur högtillgängliga virtuella datorer.

> [!div class="nextstepaction"]
> [Skapa virtuella datorer med hög tillgänglighet](tutorial-availability-sets.md)



