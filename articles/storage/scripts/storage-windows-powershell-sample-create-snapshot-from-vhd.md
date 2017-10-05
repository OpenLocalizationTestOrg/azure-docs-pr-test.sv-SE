---
title: "Azure PowerShell-skript Sample - skapa en ögonblicksbild från en virtuell Hårddisk för att skapa flera identiska hanterade diskar i tid | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en ögonblicksbild från en virtuell Hårddisk för att skapa flera identiska hanterade diskar i tid"
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
ms.openlocfilehash: 8588a33a1f0b4cce0059a40239301587cdc28920
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-snapshot-from-a-vhd-to-create-multiple-identical-managed-disks-in-small-amount-of-time-with-powershell"></a>Skapa en ögonblicksbild från en virtuell Hårddisk för att skapa flera identiska hanterade diskar i tid med PowerShell

Det här skriptet skapar en ögonblicksbild från en VHD-fil i ett lagringskonto i samma eller en annan prenumeration. Använd det här skriptet för att importera ett speciellt (inte generaliserad/Sysprep) virtuell Hårddisk till en ögonblicksbild och använder sedan ögonblicksbilden för att skapa flera identiska hanteras diskarna i tid. Använd det att importera data VHD till en ögonblicksbild och sedan använda ögonblicksbilden för att skapa flera hanterade diskar i tid. 

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[huvudsakliga](../../../powershell_scripts/storage/create-snapshots-from-vhd-in-different-subscription/create-snapshots-from-vhd-in-different-subscription.ps1 "skapa ögonblicksbilder från VHD")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa en hanterad disk från en virtuell Hårddisk i annan prenumeration. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Skapar diskkonfigurationen som används för att skapa en disk. Den omfattar lagringstyp, plats, resurs-Id för det lagringskonto där överordnat virtuella Hårddisken lagras och VHD-URI för överordnat virtuella Hårddisken. |
| [Ny AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades. |

## <a name="next-steps"></a>Nästa steg

[Skapa en hanterad disk från en ögonblicksbild](./../../storage/scripts/storage-windows-powershell-sample-create-managed-disk-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)


[Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).