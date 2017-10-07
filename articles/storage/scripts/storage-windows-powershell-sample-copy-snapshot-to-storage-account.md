---
title: "aaaAzure skriptexempel PowerShell - Export/kopiera ögonblicksbild som VHD tooa storage-konto i en annan region | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Export/kopiera ögonblicksbild som VHD tooa lagringskonto i samma annan region"
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
ms.openlocfilehash: 3b3e38c6b06bfa1e117f4e913dfc09443a795196
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-powershell"></a>Export/kopiera hanterad ögonblicksbilder som VHD tooa storage-konto i en annan region med PowerShell

Det här skriptet exporterar ett hanterade ögonblicksbild tooa storage-konto i annan region. Den första genererar hello hello ögonblicksbild SAS-URI och använder sedan toocopy den tooa storage-konto i en annan region. Använda skript toomaintain säkerhetskopian hanterade diskar i annan region för katastrofåterställning.  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toogenerate SAS-URI för en hanterad ögonblicksbilder och kopior och hello ögonblicksbild tooa lagringskonto med hjälp av SAS-URI. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Bevilja AzureRmSnapshotAccess](/powershell/module/azurerm.compute/New-AzureRmDisk) | Genererar SAS-URI för en ögonblicksbild som är används toocopy den tooa storage-konto. |
| [Ny AzureStorageContext](/powershell/module/azure.storage/New-AzureStorageContext) | Skapar en kontexten för lagringskontot med hello kontonamnet och nyckeln. Den här kontexten kan vara används tooperform läsning och skrivning åtgärder på hello storage-konto. |
| [Start-AzureStorageBlobCopy](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | Kopior hello underliggande VHD från en ögonblicksbild tooa storage-konto |

## <a name="next-steps"></a>Nästa steg

[Skapa en hanterad disk från en virtuell Hårddisk](./../scripts/storage-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[Skapa en virtuell dator från en hanterad disk](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
