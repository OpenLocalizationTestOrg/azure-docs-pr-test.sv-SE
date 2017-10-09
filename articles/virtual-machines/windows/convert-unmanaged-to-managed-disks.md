---
title: "aaaConvert Windows-dator från ohanterade diskar toomanaged diskar - hanterade diskar i Azure | Microsoft Docs"
description: "Hur tooconvert en Windows-VM från ohanterade diskar toomanaged diskar med PowerShell i hello Resource Manager-distributionsmodellen"
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
ms.openlocfilehash: e8ed8694b0e776d22df26261e2fc8340bfe5cafa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a><span data-ttu-id="84d5d-103">Konvertera en virtuell Windows-dator från ohanterade diskar toomanaged diskar</span><span class="sxs-lookup"><span data-stu-id="84d5d-103">Convert a Windows virtual machine from unmanaged disks toomanaged disks</span></span>

<span data-ttu-id="84d5d-104">Om du har befintliga virtuella Windows-datorer (VM) som använder ohanterade diskar, kan du konvertera hello VMs toouse hanterade diskar via hello [Azure hanterade diskar](managed-disks-overview.md) service.</span><span class="sxs-lookup"><span data-stu-id="84d5d-104">If you have existing Windows virtual machines (VMs) that use unmanaged disks, you can convert hello VMs toouse managed disks through hello [Azure Managed Disks](managed-disks-overview.md) service.</span></span> <span data-ttu-id="84d5d-105">Den här processen konverterar hello OS-disk- och eventuella anslutna hårddiskar.</span><span class="sxs-lookup"><span data-stu-id="84d5d-105">This process converts both hello OS disk and any attached data disks.</span></span>

<span data-ttu-id="84d5d-106">Den här artikeln beskrivs hur du tooconvert virtuella datorer med hjälp av Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="84d5d-106">This article shows you how tooconvert VMs by using Azure PowerShell.</span></span> <span data-ttu-id="84d5d-107">Om du behöver tooinstall eller uppgradera den [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span><span class="sxs-lookup"><span data-stu-id="84d5d-107">If you need tooinstall or upgrade it, see [Install and configure Azure PowerShell](/powershell/azure/install-azurerm-ps.md).</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="84d5d-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="84d5d-108">Before you begin</span></span>


* <span data-ttu-id="84d5d-109">Granska [planera för migrering av hello tooManaged diskar](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span><span class="sxs-lookup"><span data-stu-id="84d5d-109">Review [Plan for hello migration tooManaged Disks](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).</span></span>

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a><span data-ttu-id="84d5d-110">Konvertera single instance virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="84d5d-110">Convert single-instance VMs</span></span>
<span data-ttu-id="84d5d-111">Det här avsnittet beskriver hur tooconvert single instance Azure virtuella datorer från ohanterade diskar toomanaged diskar.</span><span class="sxs-lookup"><span data-stu-id="84d5d-111">This section covers how tooconvert single-instance Azure VMs from unmanaged disks toomanaged disks.</span></span> <span data-ttu-id="84d5d-112">(Om dina virtuella datorer i en tillgänglighetsuppsättning, se hello nästa avsnitt.)</span><span class="sxs-lookup"><span data-stu-id="84d5d-112">(If your VMs are in an availability set, see hello next section.)</span></span> 

1. <span data-ttu-id="84d5d-113">Frigöra hello VM med hjälp av hello [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="84d5d-113">Deallocate hello VM by using hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet.</span></span> <span data-ttu-id="84d5d-114">hello följande exempel tar bort hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="84d5d-114">hello following example deallocates hello VM named `myVM` in hello resource group named `myResourceGroup`:</span></span> 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. <span data-ttu-id="84d5d-115">Konvertera hello VM toomanaged diskar med hjälp av hello [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="84d5d-115">Convert hello VM toomanaged disks by using hello [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet.</span></span> <span data-ttu-id="84d5d-116">följande process konverterar hello hello tidigare VM, inklusive hello OS-disk- och eventuella hårddiskar:</span><span class="sxs-lookup"><span data-stu-id="84d5d-116">hello following process converts hello previous VM, including hello OS disk and any data disks:</span></span>

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. <span data-ttu-id="84d5d-117">Starta hello VM efter hello konvertering toomanaged diskar med hjälp av [Start AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span><span class="sxs-lookup"><span data-stu-id="84d5d-117">Start hello VM after hello conversion toomanaged disks by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm).</span></span> <span data-ttu-id="84d5d-118">följande exempel omstarter hello hello tidigare VM:</span><span class="sxs-lookup"><span data-stu-id="84d5d-118">hello following example restarts hello previous VM:</span></span>

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a><span data-ttu-id="84d5d-119">Konvertera virtuella datorer i en tillgänglighetsuppsättning</span><span class="sxs-lookup"><span data-stu-id="84d5d-119">Convert VMs in an availability set</span></span>

<span data-ttu-id="84d5d-120">Om hello virtuella datorer som du vill tooconvert toomanaged diskar är i en tillgänglighetsuppsättning, måste du först tooconvert hello tillgänglighet set tooa hanteras tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="84d5d-120">If hello VMs that you want tooconvert toomanaged disks are in an availability set, you first need tooconvert hello availability set tooa managed availability set.</span></span>

1. <span data-ttu-id="84d5d-121">Konvertera hello tillgänglighetsuppsättning med hjälp av hello [uppdatering AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="84d5d-121">Convert hello availability set by using hello [Update-AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet.</span></span> <span data-ttu-id="84d5d-122">följande exempel uppdateringar hello tillgänglighetsuppsättning namngivna hello `myAvailabilitySet` i hello resursgrupp med namnet `myResourceGroup`:</span><span class="sxs-lookup"><span data-stu-id="84d5d-122">hello following example updates hello availability set named `myAvailabilitySet` in hello resource group named `myResourceGroup`:</span></span>

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  <span data-ttu-id="84d5d-123">Om hello region där din tillgänglighetsuppsättning finns har bara 2 hanterade feldomäner men hello antalet ohanterade feldomäner är 3, det här kommandot visar ett fel liknande för angivna ”hello feldomänsantalet 3 måste ligga i hello intervallet 1 too2”.</span><span class="sxs-lookup"><span data-stu-id="84d5d-123">If hello region where your availability set is located has only 2 managed fault domains but hello number of unmanaged fault domains is 3, this command shows an error similar too"hello specified fault domain count 3 must fall in hello range 1 too2."</span></span> <span data-ttu-id="84d5d-124">tooresolve hello fel, uppdatering hello fel domän too2 och uppdatera `Sku` för`Aligned` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="84d5d-124">tooresolve hello error, update hello fault domain too2 and update `Sku` too`Aligned` as follows:</span></span>

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. <span data-ttu-id="84d5d-125">Frigöra och konvertera hello virtuella datorer i hello tillgänglighetsuppsättning.</span><span class="sxs-lookup"><span data-stu-id="84d5d-125">Deallocate and convert hello VMs in hello availability set.</span></span> <span data-ttu-id="84d5d-126">hello följande skript tar bort varje virtuell dator med hjälp av hello [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, konverterar den med hjälp av [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), och startar om den med hjälp av [Start AzureRmVM ](/powershell/module/azurerm.compute/start-azurermvm):</span><span class="sxs-lookup"><span data-stu-id="84d5d-126">hello following script deallocates each VM by using hello [Stop-AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, converts it by using [ConvertTo-AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), and restarts it by using [Start-AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm):</span></span>

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


## <a name="troubleshooting"></a><span data-ttu-id="84d5d-127">Felsökning</span><span class="sxs-lookup"><span data-stu-id="84d5d-127">Troubleshooting</span></span>

<span data-ttu-id="84d5d-128">Om det uppstår ett fel under konverteringen, eller om en virtuell dator är i ett felaktigt tillstånd på grund av problem i en tidigare konvertering, kör hello `ConvertTo-AzureRmVMManagedDisk` cmdlet igen.</span><span class="sxs-lookup"><span data-stu-id="84d5d-128">If there is an error during conversion, or if a VM is in a failed state because of issues in a previous conversion, run hello `ConvertTo-AzureRmVMManagedDisk` cmdlet again.</span></span> <span data-ttu-id="84d5d-129">En enkel försök avblockeras vanligtvis hello situation.</span><span class="sxs-lookup"><span data-stu-id="84d5d-129">A simple retry usually unblocks hello situation.</span></span>


## <a name="next-steps"></a><span data-ttu-id="84d5d-130">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84d5d-130">Next steps</span></span>

[<span data-ttu-id="84d5d-131">Konvertera hanterade standarddiskar toopremium</span><span class="sxs-lookup"><span data-stu-id="84d5d-131">Convert standard managed disks toopremium</span></span>](convert-disk-storage.md)

<span data-ttu-id="84d5d-132">Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).</span><span class="sxs-lookup"><span data-stu-id="84d5d-132">Take a read-only copy of a VM by using [snapshots](snapshot-copy-managed-disk.md).</span></span>

