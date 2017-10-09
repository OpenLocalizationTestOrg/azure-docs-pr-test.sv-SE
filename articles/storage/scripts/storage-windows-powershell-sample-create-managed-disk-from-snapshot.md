---
title: "aaaAzure PowerShell skriptexempel - skapa en hanterad disk från en ögonblicksbild | Microsoft Docs"
description: "Azure PowerShell-skript Sample - skapa en hanterad disk från en ögonblicksbild"
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
ms.openlocfilehash: 4fa34a8d6c67171083fba9a9ad73ecca5e0f0229
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-snapshot-with-powershell"></a>Skapa en hanterad disk från en ögonblicksbild med PowerShell

Det här skriptet skapar en hanterad disk från en ögonblicksbild. Använd den toorestore en virtuell dator från ögonblicksbilder av Operativsystemet och datadiskarna. Skapa OS och data från respektive ögonblicksbilder-hanterade diskar och sedan skapa en ny virtuell dator genom att koppla hanterade diskar. Du kan även återställa datadiskar på en befintlig virtuell dator genom att koppla datadiskar som skapas med hjälp av ögonblicksbilder.

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/storage/create-managed-disk-from-snapshot/create-managed-disk-from-snapshot.ps1 "Create managed disk from snapshot")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hanterade diskar från en ögonblicksbild. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Get-AzureRmSnapshot](/powershell/module/azurerm.compute/Get-AzureRmSnapshot) | Hämtar ögonblicksbild egenskaper.  |
| [Ny AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Skapar diskkonfigurationen som används för att skapa en disk. Den omfattar hello resurs-Id för hello överordnade ögonblicksbilden plats som är samma som hello platsen för överordnade ögonblicksbild och hello lagringstyp.  |
| [Ny AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades. |


## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator från en hanterad disk](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
