---
title: aaaUse PowerShell tooresize en Windows-dator i Azure | Microsoft Docs
description: "Ändra storlek på en Windows-dator som skapats i hello Resource Manager-distributionsmodellen, med hjälp av Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 057ff274-6dad-415e-891c-58f8eea9ed78
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: a4a80f3bc99911e4f1a095f0ce63aca00fa50694
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm"></a>Ändra storlek på en Windows VM
Den här artikeln visar hur tooresize en Windows-VM skapas i hello Resource Manager-distributionsmodellen med hjälp av Azure Powershell.

När du har skapat en virtuell dator (VM) kan du skala hello VM uppåt eller nedåt genom att ändra hello VM-storlek. I vissa fall måste du frigöra hello VM först. Detta kan inträffa om nya hello-storleken inte är tillgänglig på hello maskinvara kluster som för närvarande är värd hello VM.

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a>Ändra storlek på en Windows-VM inte i en tillgänglighetsuppsättning
1. Lista över hello VM-storlekar som är tillgängliga på hello maskinvara kluster där hello VM finns. 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. Kör följande kommandon tooresize hello VM hello eventuellt hello storlek visas. Eventuellt hello storleken inte finns går du vidare toostep 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Kör hello följande kommandon toodeallocate hello VM, ändra storlek på den och starta om hello VM eventuellt hello storlek inte visas.
   
    ```powershell
    $rgname = "<resourceGroupName>"
    $vmname = "<vmName>"
    Stop-AzureRmVM -ResourceGroupName $rgname -VMName $vmname -Force
    $vm = Get-AzureRmVM -ResourceGroupName $rgname -VMName $vmname
    $vm.HardwareProfile.VmSize = "<newVMSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName $rgname
    Start-AzureRmVM -ResourceGroupName $rgname -Name $vmname
    ```

> [!WARNING]
> Det har frigjorts hello VM Frigör alla dynamiska IP-adresser som tilldelats toohello VM. hello OS- och datadiskar påverkas inte. 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a>Ändra storlek på en virtuell Windows-dator i en tillgänglighetsuppsättning
Om hello nya storleken för en virtuell dator i en tillgänglighetsuppsättning inte är tillgänglig på hello maskinvara klustret måste för närvarande värd hello VM sedan alla virtuella datorer i tillgänglighetsuppsättningen hello toobe frigjorts tooresize hello VM. Du kanske också måste tooupdate hello storleken på andra virtuella datorer i hello tillgänglighetsuppsättning när en virtuell dator har storleksändrats. tooresize en virtuell dator i en tillgänglighetsuppsättning utföra hello följande steg.

1. Lista över hello VM-storlekar som är tillgängliga på hello maskinvara kluster där hello VM finns.
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. Kör följande kommandon tooresize hello VM hello eventuellt hello storlek visas. Om det inte finns med i listan går toostep 3.
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. Eventuellt hello storleken inte finns med i listan, fortsätter du med följande steg toodeallocate hello alla virtuella datorer i tillgänglighetsuppsättningen hello, ändra storlek på virtuella datorer och starta om dem.
4. Stoppa alla virtuella datorer i tillgänglighetsuppsättningen hello.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     Stop-AzureRmVM -ResourceGroupName $rg -Name $vmName -Force
   } 
   ```
5. Ändra storlek och starta om hello virtuella datorer i tillgänglighetsuppsättningen hello.
   
   ```powershell
   $rg = "<resourceGroupName>"
   $newSize = "<newVmSize>"
   $as = Get-AzureRmAvailabilitySet -ResourceGroupName $rg
   $vmIds = $as.VirtualMachinesReferences
   foreach ($vmId in $vmIDs){
     $string = $vmID.Id.Split("/")
     $vmName = $string[8]
     $vm = Get-AzureRmVM -ResourceGroupName $rg -Name $vmName
     $vm.HardwareProfile.VmSize = $newSize
     Update-AzureRmVM -ResourceGroupName $rg -VM $vm
     Start-AzureRmVM -ResourceGroupName $rg -Name $vmName
   }
   ```

## <a name="next-steps"></a>Nästa steg
* För ytterligare utbyggbarhet köra flera VM-instanser och skala ut. Mer information finns i [skala automatiskt Windows-datorer i en virtuell dator Skaluppsättning](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).

