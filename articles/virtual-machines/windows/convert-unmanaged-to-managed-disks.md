---
title: "Konvertera en virtuell Windows-dator från ohanterade diskar till hanterade diskar - hanterade diskar i Azure | Microsoft Docs"
description: "Konvertera en virtuell Windows-dator från ohanterade diskar till hanterade diskar med PowerShell i Resource Manager-distributionsmodellen"
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
ms.date: 06/23/2017
ms.author: cynthn
ms.openlocfilehash: 54afcf1e37f696979bfe270a473c72aedf20dc43
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-to-managed-disks"></a><span data-ttu-id="0cdfc-103">Konvertera en virtuell Windows-dator från ohanterade diskar till hanterade diskar</span><span class="sxs-lookup"><span data-stu-id="0cdfc-103">Convert a Windows virtual machine from unmanaged disks to managed disks</span></span>

<span data-ttu-id="0cdfc-104">Om du har befintliga virtuella Windows-datorer (VM) som använder ohanterade diskar, kan du konvertera virtuella datorer om du vill använda hanterade diskar via den [Azure hanterade diskar](managed-disks-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-104">If you have existing Windows virtual machines (VMs) that use unmanaged disks, you can convert the VMs to use managed disks through the [Azure Managed Disks](managed-disks-overview.md) service.</span></span> <span data-ttu-id="0cdfc-105">Den här processen konverterar både OS-disken och eventuella anslutna hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-105">This process converts both the OS disk and any attached data disks.</span></span>

<span data-ttu-id="0cdfc-106">Den här artikeln visar hur du konverterar virtuella datorer med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-106">This article shows you how to convert VMs by using Azure PowerShell.</span></span> <span data-ttu-id="0cdfc-107">Om du behöver installera eller uppgradera den, se [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="0cdfc-107">If you need to install or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="0cdfc-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="0cdfc-108">Before you begin</span></span>


* <span data-ttu-id="0cdfc-109">Granska [planera för migrering till hanterade diskar](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span><span class="sxs-lookup"><span data-stu-id="0cdfc-109">Review [Plan for the migration to Managed Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a><span data-ttu-id="0cdfc-110">Konvertera single instance virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="0cdfc-110">Convert single-instance VMs</span></span>
<span data-ttu-id="0cdfc-111">Det här avsnittet beskriver hur du konverterar single instance virtuella Azure-datorer från ohanterade diskar till hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-111">This section covers how to convert single-instance Azure VMs from unmanaged disks to managed disks.</span></span> <span data-ttu-id="0cdfc-112">(Om dina virtuella datorer i en tillgänglighetsuppsättning, finns i nästa avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="0cdfc-112">(If your VMs are in an availability set, see the next section.)</span></span> 

1. <span data-ttu-id="0cdfc-113">Frigör den virtuella datorn med hjälp av den [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-113">Deallocate the VM by using the [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span></span> <span data-ttu-id="0cdfc-114">I följande exempel tar bort den virtuella datorn med namnet `myVM` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0cdfc-114">The following example deallocates the VM named `myVM` in the resource group named `myResourceGroup`:</span></span> 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. <span data-ttu-id="0cdfc-115">Konvertera den virtuella datorn till hanterade diskar med hjälp av den [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-115">Convert the VM to managed disks by using the [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span></span> <span data-ttu-id="0cdfc-116">Följande process konverterar tidigare VM, inklusive OS-disken och eventuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="0cdfc-116">The following process converts the previous VM, including the OS disk and any data disks:</span></span>

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. <span data-ttu-id="0cdfc-117">Starta den virtuella datorn efter konvertering till hanterade diskar med hjälp av [Start AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="0cdfc-117">Start the VM after the conversion to managed disks by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span></span> <span data-ttu-id="0cdfc-118">I följande exempel startar om den tidigare:</span><span class="sxs-lookup"><span data-stu-id="0cdfc-118">The following example restarts the previous VM:</span></span>

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="0cdfc-119">Konvertera virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="0cdfc-119">Convert VMs in an availability set</span></span>

<span data-ttu-id="0cdfc-120">Om de virtuella datorer som du vill konvertera till hanterade diskar är i en tillgänglighetsuppsättning, måste du först konvertera tillgänglighetsuppsättning till en hanterad tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-120">If the VMs that you want to convert to managed disks are in an availability set, you first need to convert the availability set to a managed availability set.</span></span>

1. <span data-ttu-id="0cdfc-121">Konvertera tillgänglighetsuppsättning med hjälp av den [uppdatering AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-121">Convert the availability set by using the [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span></span> <span data-ttu-id="0cdfc-122">I följande exempel uppdateras tillgänglighetsuppsättning namngivna `myAvailabilitySet` i resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="0cdfc-122">The following example updates the availability set named `myAvailabilitySet` in the resource group named `myResourceGroup`:</span></span>

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  <span data-ttu-id="0cdfc-123">Om den region där din tillgänglighetsuppsättning finns har bara 2 hanterade feldomäner men antalet ohanterade feldomäner 3, det här kommandot visar ett fel liknar ”det angivna feldomänsantalet 3 måste ligga i intervallet 1 till 2”.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-123">If the region where your availability set is located has only 2 managed fault domains but the number of unmanaged fault domains is 3, this command shows an error similar to "The specified fault domain count 3 must fall in the range 1 to 2."</span></span> <span data-ttu-id="0cdfc-124">Åtgärda felet genom att uppdatera feldomänen till 2 och uppdatera `Sku` till `Aligned` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0cdfc-124">To resolve the error, update the fault domain to 2 and update `Sku` to `Aligned` as follows:</span></span>

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. <span data-ttu-id="0cdfc-125">Frigöra och konvertera virtuella datorer i tillgänglighetsuppsättningen.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-125">Deallocate and convert the VMs in the availability set.</span></span> <span data-ttu-id="0cdfc-126">Följande skript tar bort varje virtuell dator med hjälp av den [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, konverterar den med hjälp av [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), och startar om den med hjälp av [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="0cdfc-126">The following script deallocates each VM by using the [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converts it by using [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), and restarts it by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

  ```powershell
  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName

  foreach($vmInfo in $avSet.VirtualMachinesReferences)
  {
     $vm = Get-AzureRmVM -ResourceGroupName $rgName | Where-Object {$_.Id -eq $vmInfo.id}
     Stop-AzureRmVM -ResourceGroupName $rgName -Name $vm.Name -Force
     ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vm.Name
     Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  }
  ```


## <a name="troubleshooting"></a><span data-ttu-id="0cdfc-127">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0cdfc-127">Troubleshooting</span></span>

<span data-ttu-id="0cdfc-128">Om det uppstår ett fel under konverteringen, eller om en virtuell dator är i ett felaktigt tillstånd på grund av problem i en tidigare konvertering, kör den `ConvertTo-AzureRmVMManagedDisk` cmdlet igen.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-128">If there is an error during conversion, or if a VM is in a failed state because of issues in a previous conversion, run the `ConvertTo-AzureRmVMManagedDisk` cmdlet again.</span></span> <span data-ttu-id="0cdfc-129">En enkel försök avblockeras vanligtvis situationen.</span><span class="sxs-lookup"><span data-stu-id="0cdfc-129">A simple retry usually unblocks the situation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="0cdfc-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0cdfc-130">Next steps</span></span>

[<span data-ttu-id="0cdfc-131">Konvertera hanterade standarddiskar till premium</span><span class="sxs-lookup"><span data-stu-id="0cdfc-131">Convert standard managed disks to premium</span></span>](convert-disk-storage.md)

<span data-ttu-id="0cdfc-132">Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="0cdfc-132">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

