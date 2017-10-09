---
title: "aaaAvailability anger självstudier för virtuella Windows-datorer i Azure | Microsoft Docs"
description: "Läs mer om hello tillgänglighet uppsättningar för virtuella Windows-datorer i Azure."
documentationcenter: 
services: virtual-machines-windows
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
ms.date: 05/08/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 853775c5f126dd815c1933f9d71d2274a75ea661
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-availability-sets"></a>Hur toouse tillgänglighetsuppsättningar

I kursen får du lära dig hur tooincrease hello tillgänglighet och tillförlitlighet för dina lösningar för virtuell dator på Azure med hjälp av en funktion kallad Tillgänglighetsuppsättningar. Tillgänglighetsuppsättningar se till att hello virtuella datorer som du distribuerar i Azure är fördelade på flera isolerade maskinvara kluster. Detta säkerställer att om ett maskinvaru- eller fel i Azure inträffar, underordnade uppsättning dina virtuella datorer som påverkas och som din lösning ska vara tillgängliga och fungerar ur hello av dina kunder som använder den. 

I den här guiden får du lära dig hur man:

> [!div class="checklist"]
> * Skapa en tillgänglighetsuppsättning
> * Skapa en virtuell dator i en tillgänglighetsuppsättning
> * Kontrollera tillgängliga storlekar på VM

Den här kursen kräver hello Azure PowerShell module 3,6 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooupgrade finns [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).

## <a name="availability-set-overview"></a>Översikt över tillgänglighetsuppsättning

En Tillgänglighetsuppsättning är en logisk gruppering funktion som du kan använda i Azure tooensure att hello VM resurser som du placerar i den isoleras från varandra när de distribueras i ett Azure-datacenter. Azure garanterar att hello virtuella datorer du placera inom en Tillgänglighetsuppsättning körs över flera fysiska servrar, beräkna rack, lagringsenheter och nätverksväxlar. Detta säkerställer att hello för händelsen maskinvaru- eller programvarufel Azure, påverkas endast en delmängd av dina virtuella datorer, och tillämpningsprogrammet övergripande förblir upp och fortsätta toobe tillgängliga tooyour kunder. Med hjälp av Tillgänglighetsuppsättningar är en viktig funktion tooleverage om du vill toobuild tillförlitliga molnlösningar.

Nu ska vi titta en typisk VM-baserad lösning där du kan ha 4 frontend-webbservrar och använda 2 backend-VMs som värd för en databas. Med Azure, du kan toodefine två tillgänglighetsuppsättningar innan du distribuerar dina virtuella datorer: en tillgänglighetsuppsättning för hello ”web”-nivå och en tillgänglighetsuppsättning för hello ”databas” nivå. När du skapar en ny virtuell dator kan du ange hello tillgänglighet som en parameter toohello az virtuell dator skapar kommando och Azure automatiskt garanterar att hello virtuella datorer som du skapar inom hello tillgänglig separat uppsättning över flera fysiska maskinvaruresurser. Det innebär att du vet att hello om hello fysiska maskinvara som din webbserver eller databasen Server virtuella datorer körs på ett problem har uppstått, andra instanser av webbservern och databasen virtuella datorer fortsätter att köras bra eftersom de finns på olika typer av maskinvara.

Du bör alltid använda Tillgänglighetsuppsättningar när du vill toodeploy tillförlitliga VM baserade lösningar i Azure.

## <a name="create-an-availability-set"></a>Skapa en tillgänglighetsuppsättning

Du kan skapa en tillgänglighetsuppsättning med [ny AzureRmAvailabilitySet](/powershell/module/azurerm.compute/new-azurermavailabilityset). I det här exemplet vi ange båda hello antalet domäner för uppdatering och fel på *2* för hello tillgänglighetsuppsättning namngivna *myAvailabilitySet* i hello *myResourceGroupAvailability*resursgruppen.

Skapa en resursgrupp.

```powershell
New-AzureRmResourceGroup -Name myResourceGroupAvailability -Location EastUS
```


```powershell
New-AzureRmAvailabilitySet `
   -Location EastUS `
   -Name myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability `
   -Managed `
   -PlatformFaultDomainCount 2 `
   -PlatformUpdateDomainCount 2
```

## <a name="create-vms-inside-an-availability-set"></a>Skapa virtuella datorer i en tillgänglighetsuppsättning

Virtuella datorer måste toobe skapas i hello tillgänglighet set toomake till korrekt distribueras de över hello maskinvara. Du kan inte lägga till en befintlig virtuell dator tooan tillgänglighetsuppsättning när den har skapats. 

hello maskinvara på en plats är uppdelat i toomultiple uppdatering domäner och feldomäner. En **uppdateringsdomän** är en grupp virtuella datorer och underliggande fysiska maskinvara som kan startas på hello samtidigt. Virtuella datorer i hello samma **feldomän** delar vanliga storage samt en gemensam käll- och strömbrytare. 

När du skapar en virtuell dator med hjälp av konfiguration av [ny AzureRMVMConfig](/powershell/module/azurerm.compute/new-azurermvmconfig) du ange hello tillgänglighet anges med hello `-AvailabilitySetId` parametern toospecify hello-ID för hello tillgänglighetsuppsättning.

Skapa 2 virtuella datorer med [ny AzureRmVM](/powershell/module/azurerm.compute/new-azurermvm) i hello tillgänglighetsuppsättningen.

```powershell
$availabilitySet = Get-AzureRmAvailabilitySet `
    -ResourceGroupName myResourceGroupAvailability `
    -Name myAvailabilitySet

$cred = Get-Credential -Message "Enter a username and password for hello virtual machine."

$subnetConfig = New-AzureRmVirtualNetworkSubnetConfig `
    -Name mySubnet `
    -AddressPrefix 192.168.1.0/24
$vnet = New-AzureRmVirtualNetwork `
    -ResourceGroupName myResourceGroupAvailability `
    -Location EastUS `
    -Name MYvNET `
    -AddressPrefix 192.168.0.0/16 `
    -Subnet $subnetConfig

for ($i=1; $i -le 2; $i++)
{
   $pip = New-AzureRmPublicIpAddress `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name "mypublicdns$(Get-Random)" `
        -AllocationMethod Static `
        -IdleTimeoutInMinutes 4

   $nsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig `
        -Name myNetworkSecurityGroupRuleRDP$i `
        -Protocol Tcp `
        -Direction Inbound `
        -Priority 1000 `
        -SourceAddressPrefix * `
        -SourcePortRange * `
        -DestinationAddressPrefix * `
        -DestinationPortRange 3389 `
        -Access Allow

   $nsg = New-AzureRmNetworkSecurityGroup `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -Name myNetworkSecurityGroup$i `
        -SecurityRules $nsgRuleRDP

   $nic = New-AzureRmNetworkInterface `
        -Name myNic$i `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -SubnetId $vnet.Subnets[0].Id `
        -PublicIpAddressId $pip.Id `
        -NetworkSecurityGroupId $nsg.Id

   # Here is where we specify hello availability set
   $vm = New-AzureRmVMConfig `
        -VMName myVM$i `
        -VMSize Standard_D1 `
        -AvailabilitySetId $availabilitySet.Id

   $vm = Set-AzureRmVMOperatingSystem `
        -VM $vm `
        -Windows -ComputerName myVM$i `   
        -Credential $cred `
        -ProvisionVMAgent `
        -EnableAutoUpdate
   $vm = Set-AzureRmVMSourceImage `
        -VM $vm `
        -PublisherName MicrosoftWindowsServer `
        -Offer WindowsServer `
        -Skus 2016-Datacenter `
        -Version latest
   $vm = Set-AzureRmVMOSDisk `
        -VM $vm `
        -Name myOsDisk$i `
        -DiskSizeInGB 128 `
        -CreateOption FromImage `
        -Caching ReadWrite
   $vm = Add-AzureRmVMNetworkInterface -VM $vm -Id $nic.Id
   New-AzureRmVM `
        -ResourceGroupName myResourceGroupAvailability `
        -Location EastUS `
        -VM $vm
}

```

Det tar några minuter toocreate och konfigurera både virtuella datorer. När du är klar måste 2 virtuella datorer distribuerade i hello underliggande maskinvara. 

## <a name="check-for-available-vm-sizes"></a>Sök efter tillgängliga storlekar på VM 

Du kan lägga till flera virtuella datorer toohello tillgänglighetsuppsättning senare, men du måste tooknow vilka VM-storlekar är tillgängliga på hello maskinvara. Använd [Get-AzureRMVMSize](/powershell/module/azurerm.compute/get-azurermvmsize) toolist alla hello tillgängliga storlekar på hello maskinvara kluster för hello tillgänglighetsuppsättning.

```powershell
Get-AzureRmVMSize `
   -AvailabilitySetName myAvailabilitySet `
   -ResourceGroupName myResourceGroupAvailability  
```

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen lärde du dig att:

> [!div class="checklist"]
> * Skapa en tillgänglighetsuppsättning
> * Skapa en virtuell dator i en tillgänglighetsuppsättning
> * Kontrollera tillgängliga storlekar på VM

Avancera toohello nästa självstudiekurs toolearn om skalningsuppsättningar i virtuella datorer.

> [!div class="nextstepaction"]
> [Skapa en VM-skalningsuppsättning](tutorial-create-vmss.md)


