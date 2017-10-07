---
title: aaaAzure PowerShell skriptexempel - kopia (flytta) hanteras diskar toosame eller annan prenumeration | Microsoft Docs
description: Azure PowerShell skriptexempel - kopia (flytta) hanteras diskar toosame eller en annan prenumeration
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
ms.date: 06/06/2017
ms.author: ramankum
ms.openlocfilehash: 22e19a47228cbf628bebebd73012b8aa7baf073c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-in-hello-same-subscription-or-different-subscription-with-powershell"></a>Kopiera hanterade diskar i hello samma prenumeration eller annan prenumeration med PowerShell

Det här skriptet skapar en kopia av en befintlig hanterade disk i hello samma prenumeration eller en annan prenumeration. hello ny disk skapas i hello samma region som hello överordnade hanterade disken.   

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-powershell[main](../../../powershell_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.ps1 "Copy managed disk")]


## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate en ny hanterade disk i hello mål en prenumeration med hjälp av hello-Id för hello källa hanterade disken. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Ny AzureRmDiskConfig](/powershell/module/azurerm.compute/New-AzureRmDiskConfig) | Skapar diskkonfigurationen som används för att skapa en disk. Den omfattar hello resurs-Id för hello överordnad disk och plats som är samma som hello platsen för överordnad disk.  |
| [Ny AzureRmDisk](/powershell/module/azurerm.compute/New-AzureRmDisk) | Skapar en disk med diskkonfigurationen disknamnet och resursgruppens namn som parametrar skickades. |


## <a name="next-steps"></a>Nästa steg

[Skapa en virtuell dator från en hanterad disk](./virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).

Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../app-service-web/app-service-powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
