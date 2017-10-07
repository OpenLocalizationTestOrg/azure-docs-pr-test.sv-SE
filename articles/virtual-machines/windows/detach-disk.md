---
title: "aaaDetach en datadisk från en Windows-VM - Azure | Microsoft Docs"
description: "Lär dig toodetach en datadisk från en virtuell dator i Azure med hjälp av hello Resource Manager-modellen."
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 13180343-ac49-4a3a-85d8-0ead95e2028c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/21/2017
ms.author: cynthn
ms.openlocfilehash: f3f581d3f33329db2ecb7d25a68bc59af7361aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodetach-a-data-disk-from-a-windows-virtual-machine"></a>Hur toodetach en disk från en Windows-dator
När du inte längre behöver en datadisk som är bifogade tooa virtuell dator kan du enkelt kan koppla från den. Detta tar bort hello disk från hello virtuell dator, men ta bort inte den från lagring.

> [!WARNING]
> Om du koppla från en disk som den inte tas bort automatiskt. Om du har prenumererat tooPremium lagring, fortsätter tooincur lagringskostnaderna för hello disk. Mer information finns för[priser och fakturering när du använder Premiumlagring](../../storage/common/storage-premium-storage.md#pricing-and-billing).
>
>

Om du vill toouse hello befintliga data på hello disk, du kan återansluta den toohello samma virtuella dator eller en annan.

## <a name="detach-a-data-disk-using-hello-portal"></a>Koppla från en datadisk med hjälp av hello portal
1. I portalen hello-hubb väljer **virtuella datorer**.
2. Välj hello virtuella dator som har hello datadisk toodetach och på **stoppa** toodeallocate hello VM.
3. Hello virtuella bladet välj **diskar**.
4. Hello överst i hello **diskar** bladet väljer **redigera**.
5. I hello **diskar** bladet toohello högerkanten på hello datadisk som du vill att toodetach, klicka på hello ![Detach bilden](./media/detach-disk/detach.png) frånkoppling knappen.
5. Klicka på Spara hello längst upp på bladet hello när hello disken har tagits bort.
6. I hello virtuella bladet, klickar du på **översikt** och klicka sedan på hello **starta** knappen hello överst i hello bladet toorestart hello VM.



hello disk kvar i lagring men är inte längre kopplade tooa virtuell dator.

## <a name="detach-a-data-disk-using-powershell"></a>Koppla från en datadisk med hjälp av PowerShell
I det här exemplet hello första kommandot hämtar hello virtuell dator med namnet **MyVM07** i hello **RG11** resursgrupp med hello Get-AzureRmVM cmdlet. Hej kommandot lagrar hello virtuell dator i hello **$VirtualMachine** variabeln.

hello andra kommandot tar bort hello datadisk med namnet DataDisk3 från hello virtuell dator.

hello kommandot uppdaterar hello virtuella toocomplete hello processen för att ta bort datadisk hello hello tillstånd.

```powershell
$VirtualMachine = Get-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07"
Remove-AzureRmVMDataDisk -VM $VirtualMachine -Name "DataDisk3"
Update-AzureRmVM -ResourceGroupName "RG11" -Name "MyVM07" -VM $VirtualMachine
```

Mer information finns i [ta bort AzureRmVMDataDisk](/powershell/module/azurerm.compute/remove-azurermvmdatadisk).

## <a name="next-steps"></a>Nästa steg
Om du vill tooreuse hello datadisk du just [bifoga den tooanother VM](attach-managed-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)

