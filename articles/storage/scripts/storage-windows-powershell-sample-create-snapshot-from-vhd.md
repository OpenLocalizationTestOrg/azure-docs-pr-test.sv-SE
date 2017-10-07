---
title: "aaaAzure PowerShell skriptexempel - skapa en ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar i tid | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar i tid"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 0a13e399b692f32b3772add39fe5b5c023808c5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-snapshot-from-a-vhd-toocreate-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Skapa en ögonblicksbild från en VHD-toocreate flera identiska hanterade diskar i tid med PowerShell

Det här skriptet skapar en ögonblicksbild från en VHD-fil i ett lagringskonto i samma eller en annan prenumeration. Använd det här skriptet tooimport en särskild (inte generaliserad/Sysprep) VHD tooa ögonblicksbild och Använd hello ögonblicksbild toocreate flera identiska hanterade diskar i tid. Dessutom använder tooimport en ögonblicksbild VHD tooa och Använd hello ögonblicksbild toocreate flera hanterade diskar i tid. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "Create snapshot from VHD")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hanterade diskar från en virtuell Hårddisk i annan prenumeration. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Skapar diskkonfigurationen som används för att skapa en disk. Den omfattar lagringstyp, plats, resurs-Id för hello storage-konto där hello överordnade virtuella Hårddisken lagras och VHD-URI för hello överordnade virtuella Hårddisken. |
| [Ny AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades. |

## <a name="next-steps"></a>Nästa steg

[Skapa en hanterad disk från en ögonblicksbild](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
