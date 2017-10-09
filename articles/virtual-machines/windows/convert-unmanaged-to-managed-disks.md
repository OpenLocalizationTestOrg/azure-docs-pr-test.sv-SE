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
# <a name="convert-a-windows-virtual-machine-from-unmanaged-disks-toomanaged-disks"></a>Konvertera en virtuell Windows-dator från ohanterade diskar toomanaged diskar

Om du har befintliga virtuella Windows-datorer (VM) som använder ohanterade diskar, kan du konvertera hello VMs toouse hanterade diskar via hello [Azure hanterade diskar](managed-disks-overview.md) service. Den här processen konverterar hello OS-disk- och eventuella anslutna hårddiskar.

Den här artikeln beskrivs hur du tooconvert virtuella datorer med hjälp av Azure PowerShell. Om du behöver tooinstall eller uppgradera den [installera och konfigurera Azure PowerShell](/powershell/azure/install-azurerm-ps.md).

## <a name="before-you-begin"></a>Innan du börjar


* Granska [planera för migrering av hello tooManaged diskar](on-prem-to-azure.md#plan-for-the-migration-to-managed-disks).

[!INCLUDE [virtual-machines-common-convert-disks-considerations](../../../includes/virtual-machines-common-convert-disks-considerations.md)]




## <a name="convert-single-instance-vms"></a>Konvertera single instance virtuella datorer
Det här avsnittet beskriver hur tooconvert single instance Azure virtuella datorer från ohanterade diskar toomanaged diskar. (Om dina virtuella datorer i en tillgänglighetsuppsättning, se hello nästa avsnitt.) 

1. Frigöra hello VM med hjälp av hello [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet. hello följande exempel tar bort hello virtuella datorn med namnet `myVM` i hello resursgrupp med namnet `myResourceGroup`: 

  ```powershell
  $rgName = "myResourceGroup"
  $vmName = "myVM"
  Stop-AzureRmVM -ResourceGroupName $rgName -Name $vmName -Force
  ```

2. Konvertera hello VM toomanaged diskar med hjälp av hello [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk) cmdlet. följande process konverterar hello hello tidigare VM, inklusive hello OS-disk- och eventuella hårddiskar:

  ```powershell
  ConvertTo-AzureRmVMManagedDisk -ResourceGroupName $rgName -VMName $vmName
  ```

3. Starta hello VM efter hello konvertering toomanaged diskar med hjälp av [Start AzureRmVM](/powershell/module/azurerm.compute/start-azurermvm). följande exempel omstarter hello hello tidigare VM:

  ```powershell
  Start-AzureRmVM -ResourceGroupName $rgName -Name $vmName
  ```


## <a name="convert-vms-in-an-availability-set"></a>Konvertera virtuella datorer i en tillgänglighetsuppsättning

Om hello virtuella datorer som du vill tooconvert toomanaged diskar är i en tillgänglighetsuppsättning, måste du först tooconvert hello tillgänglighet set tooa hanteras tillgänglighetsuppsättning.

1. Konvertera hello tillgänglighetsuppsättning med hjälp av hello [uppdatering AzureRmAvailabilitySet](/powershell/module/azurerm.compute/update-azurermavailabilityset) cmdlet. följande exempel uppdateringar hello tillgänglighetsuppsättning namngivna hello `myAvailabilitySet` i hello resursgrupp med namnet `myResourceGroup`:

  ```powershell
  $rgName = 'myResourceGroup'
  $avSetName = 'myAvailabilitySet'

  $avSet = Get-AzureRmAvailabilitySet -ResourceGroupName $rgName -Name $avSetName
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned 
  ```

  Om hello region där din tillgänglighetsuppsättning finns har bara 2 hanterade feldomäner men hello antalet ohanterade feldomäner är 3, det här kommandot visar ett fel liknande för angivna ”hello feldomänsantalet 3 måste ligga i hello intervallet 1 too2”. tooresolve hello fel, uppdatering hello fel domän too2 och uppdatera `Sku` för`Aligned` på följande sätt:

  ```powershell
  $avSet.PlatformFaultDomainCount = 2
  Update-AzureRmAvailabilitySet -AvailabilitySet $avSet -Sku Aligned
  ```

2. Frigöra och konvertera hello virtuella datorer i hello tillgänglighetsuppsättning. hello följande skript tar bort varje virtuell dator med hjälp av hello [stoppa AzureRmVM](/powershell/module/azurerm.compute/stop-azurermvm) cmdlet, konverterar den med hjälp av [ConvertTo AzureRmVMManagedDisk](/powershell/module/azurerm.compute/convertto-azurermvmmanageddisk), och startar om den med hjälp av [Start AzureRmVM ](/powershell/module/azurerm.compute/start-azurermvm):

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


## <a name="troubleshooting"></a>Felsökning

Om det uppstår ett fel under konverteringen, eller om en virtuell dator är i ett felaktigt tillstånd på grund av problem i en tidigare konvertering, kör hello `ConvertTo-AzureRmVMManagedDisk` cmdlet igen. En enkel försök avblockeras vanligtvis hello situation.


## <a name="next-steps"></a>Nästa steg

[Konvertera hanterade standarddiskar toopremium](convert-disk-storage.md)

Ta en skrivskyddad kopia av en virtuell dator med hjälp av [ögonblicksbilder](snapshot-copy-managed-disk.md).

