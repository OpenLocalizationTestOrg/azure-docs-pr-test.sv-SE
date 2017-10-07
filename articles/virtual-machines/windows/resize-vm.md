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
# <a name="resize-a-windows-vm"></a><span data-ttu-id="70034-103">Ändra storlek på en Windows VM</span><span class="sxs-lookup"><span data-stu-id="70034-103">Resize a Windows VM</span></span>
<span data-ttu-id="70034-104">Den här artikeln visar hur tooresize en Windows-VM skapas i hello Resource Manager-distributionsmodellen med hjälp av Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="70034-104">This article shows you how tooresize a Windows VM, created in hello Resource Manager deployment model using Azure Powershell.</span></span>

<span data-ttu-id="70034-105">När du har skapat en virtuell dator (VM) kan du skala hello VM uppåt eller nedåt genom att ändra hello VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="70034-105">After you create a virtual machine (VM), you can scale hello VM up or down by changing hello VM size.</span></span> <span data-ttu-id="70034-106">I vissa fall måste du frigöra hello VM först.</span><span class="sxs-lookup"><span data-stu-id="70034-106">In some cases, you must deallocate hello VM first.</span></span> <span data-ttu-id="70034-107">Detta kan inträffa om nya hello-storleken inte är tillgänglig på hello maskinvara kluster som för närvarande är värd hello VM.</span><span class="sxs-lookup"><span data-stu-id="70034-107">This can happen if hello new size is not available on hello hardware cluster that is currently hosting hello VM.</span></span>

## <a name="resize-a-windows-vm-not-in-an-availability-set"></a><span data-ttu-id="70034-108">Ändra storlek på en Windows-VM inte i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="70034-108">Resize a Windows VM not in an availability set</span></span>
1. <span data-ttu-id="70034-109">Lista över hello VM-storlekar som är tillgängliga på hello maskinvara kluster där hello VM finns.</span><span class="sxs-lookup"><span data-stu-id="70034-109">List hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span> 
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName> 
    ```
2. <span data-ttu-id="70034-110">Kör följande kommandon tooresize hello VM hello eventuellt hello storlek visas.</span><span class="sxs-lookup"><span data-stu-id="70034-110">If hello desired size is listed, run hello following commands tooresize hello VM.</span></span> <span data-ttu-id="70034-111">Eventuellt hello storleken inte finns går du vidare toostep 3.</span><span class="sxs-lookup"><span data-stu-id="70034-111">If hello desired size is not listed, go on toostep 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVMsize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="70034-112">Kör hello följande kommandon toodeallocate hello VM, ändra storlek på den och starta om hello VM eventuellt hello storlek inte visas.</span><span class="sxs-lookup"><span data-stu-id="70034-112">If hello desired size is not listed, run hello following commands toodeallocate hello VM, resize it, and restart hello VM.</span></span>
   
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
> <span data-ttu-id="70034-113">Det har frigjorts hello VM Frigör alla dynamiska IP-adresser som tilldelats toohello VM.</span><span class="sxs-lookup"><span data-stu-id="70034-113">Deallocating hello VM releases any dynamic IP addresses assigned toohello VM.</span></span> <span data-ttu-id="70034-114">hello OS- och datadiskar påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="70034-114">hello OS and data disks are not affected.</span></span> 
> 
> 

## <a name="resize-a-windows-vm-in-an-availability-set"></a><span data-ttu-id="70034-115">Ändra storlek på en virtuell Windows-dator i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="70034-115">Resize a Windows VM in an availability set</span></span>
<span data-ttu-id="70034-116">Om hello nya storleken för en virtuell dator i en tillgänglighetsuppsättning inte är tillgänglig på hello maskinvara klustret måste för närvarande värd hello VM sedan alla virtuella datorer i tillgänglighetsuppsättningen hello toobe frigjorts tooresize hello VM.</span><span class="sxs-lookup"><span data-stu-id="70034-116">If hello new size for a VM in an availability set is not available on hello hardware cluster currently hosting hello VM, then all VMs in hello availability set will need toobe deallocated tooresize hello VM.</span></span> <span data-ttu-id="70034-117">Du kanske också måste tooupdate hello storleken på andra virtuella datorer i hello tillgänglighetsuppsättning när en virtuell dator har storleksändrats.</span><span class="sxs-lookup"><span data-stu-id="70034-117">You also might need tooupdate hello size of other VMs in hello availability set after one VM has been resized.</span></span> <span data-ttu-id="70034-118">tooresize en virtuell dator i en tillgänglighetsuppsättning utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="70034-118">tooresize a VM in an availability set, perform hello following steps.</span></span>

1. <span data-ttu-id="70034-119">Lista över hello VM-storlekar som är tillgängliga på hello maskinvara kluster där hello VM finns.</span><span class="sxs-lookup"><span data-stu-id="70034-119">List hello VM sizes that are available on hello hardware cluster where hello VM is hosted.</span></span>
   
    ```powershell
    Get-AzureRmVMSize -ResourceGroupName <resourceGroupName> -VMName <vmName>
    ```
2. <span data-ttu-id="70034-120">Kör följande kommandon tooresize hello VM hello eventuellt hello storlek visas.</span><span class="sxs-lookup"><span data-stu-id="70034-120">If hello desired size is listed, run hello following commands tooresize hello VM.</span></span> <span data-ttu-id="70034-121">Om det inte finns med i listan går toostep 3.</span><span class="sxs-lookup"><span data-stu-id="70034-121">If it is not listed, go toostep 3.</span></span>
   
    ```powershell
    $vm = Get-AzureRmVM -ResourceGroupName <resourceGroupName> -VMName <vmName>
    $vm.HardwareProfile.VmSize = "<newVmSize>"
    Update-AzureRmVM -VM $vm -ResourceGroupName <resourceGroupName>
    ```
3. <span data-ttu-id="70034-122">Eventuellt hello storleken inte finns med i listan, fortsätter du med följande steg toodeallocate hello alla virtuella datorer i tillgänglighetsuppsättningen hello, ändra storlek på virtuella datorer och starta om dem.</span><span class="sxs-lookup"><span data-stu-id="70034-122">If hello desired size is not listed, continue with hello following steps toodeallocate all VMs in hello availability set, resize VMs, and restart them.</span></span>
4. <span data-ttu-id="70034-123">Stoppa alla virtuella datorer i tillgänglighetsuppsättningen hello.</span><span class="sxs-lookup"><span data-stu-id="70034-123">Stop all VMs in hello availability set.</span></span>
   
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
5. <span data-ttu-id="70034-124">Ändra storlek och starta om hello virtuella datorer i tillgänglighetsuppsättningen hello.</span><span class="sxs-lookup"><span data-stu-id="70034-124">Resize and restart hello VMs in hello availability set.</span></span>
   
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

## <a name="next-steps"></a><span data-ttu-id="70034-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="70034-125">Next steps</span></span>
* <span data-ttu-id="70034-126">För ytterligare utbyggbarhet köra flera VM-instanser och skala ut. Mer information finns i [skala automatiskt Windows-datorer i en virtuell dator Skaluppsättning](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span><span class="sxs-lookup"><span data-stu-id="70034-126">For additional scalability, run multiple VM instances and scale out. For more information, see [Automatically scale Windows machines in a Virtual Machine Scale Set](../../virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md).</span></span>

